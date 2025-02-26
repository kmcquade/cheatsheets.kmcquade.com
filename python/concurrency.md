# Concurrency

Concurrency options. I wrote this to help with a student that I'm tutoring.

## Multithreading

Example:

```python
import threading
import requests
from typing import List, Tuple

# This module provides four classes demonstrating concurrency:
# Each class is intended to wrap low-level concurrency primitives in a
# simpler interface using Python's built-in modules.

class ThreadWrapper:
    """
    Real world example:
    - Scanning multiple hosts/ports in parallel
    - Sending parallel HTTP requests
    - Reading logs from multiple servers.

    Shines when operations are I/O bound.
    """
    def __init__(self, func, args_list: List[Tuple]):
        # The function each thread will run
        self.func = func
        # A list of argument tuples
        self.args_list = args_list
        # Hold thread objects
        self.threads = []

    def run(self):
        for args in self.args_list:
            t = threading.Thread(target=self.func, args=args)
            t.start()
            self.threads.append(t)
        for t in self.threads:
            t.join()


def fetch_url(url: str, headers: List[str]) -> requests.Response:
    """
    Makes a GET request to the given URL.
    Prints the status code or an error.
    :param url:
    :param headers:
    :return:
    """
    try:
        resp = requests.get(url, headers)
        return resp
    except Exception as e:
        print(f"[Threaded Fetch] Error on {url}: {e}")


def thread_wrapper_example():
    # Each item in the list is (url, headers) matching the function signature.
    request_args = [
        ("https://example.com", {"User-Agent": "TestAgent/1.0"}),
        ("https://google.com",  {"User-Agent": "TestAgent/2.0"}),
        ("https://github.com",  {"User-Agent": "TestAgent/3.0"}),
    ]

    # Create the ThreadWrapper, passing in the target function and argument tuples
    wrapper = ThreadWrapper(fetch_url, request_args)
    # Run them all in parallel threads
    wrapper.run()
    
if __name__ == '__main__':
    thread_wrapper_example()

```

## Multiprocessing

Example:

```python
import hashlib
import multiprocessing
import string
import random
from typing import List, Tuple


class ProcessWrapper:
    """
    Similar to threadwrapper but uses processes.
    Useful for CPU-bound tasks or to avoid GIL limitations.

    Examples:
        - Hashing a large set of files
        - Encrypting data
        - Performing image processing across multiple cores.

    Processes bypass the GIL for true parallel CPU bound work.
    """
    def __init__(self, func, args_list: List[Tuple]):
        self.func = func
        self.args_list = args_list
        self.procs = []

    def run(self):
        # create and start a process for each argument tuple
        for args in self.args_list:
            p = multiprocessing.Process(target=self.func, args=args)
            p.start()
            self.procs.append(p)
        # Wait for all processes to finish
        for p in self.procs:
            p.join()


def hash_string(s):
    """
    Hash a single string using SHA-256.
    Prints out the string along with its hexadecimal digest.
    """
    digest = hashlib.sha256(s.encode()).hexdigest()
    print(f"'{s}' -> {digest}")

def generate_random_string(length=8):
    """
    Generate a random string of letters/digits for demonstration.
    """
    chars = string.ascii_letters + string.digits
    return ''.join(random.choice(chars) for _ in range(length))


def process_wrapper_example():
    # Create 10 random strings
    random_strings = [generate_random_string() for _ in range(10)]

    # Build argument tuples for the ProcessWrapper (single-arg function)
    args_list = [(s,) for s in random_strings]

    # Run each hashing in its own process
    wrapper = ProcessWrapper(hash_string, args_list)
    wrapper.run()

if __name__ == '__main__':
    process_wrapper_example()

```

## Producer/Consumer Pattern

```python
import queue
import threading
import time


class ProducerConsumer:
    """
    Real world example:
    - Ingesting messages into a queue (producer) and processing them (consumer) in real time
    - Building a pipeline that downloads data (producer) and saves it to a database (consumer) to
    decouple workload stages and control flow.
    """
    def __init__(self):
        # Thread-safe FIFO queue
        self.q = queue.Queue()

    def produce(self, items):
        """
        Put items in a queue
        """
        for item in items:
            print(f"Producing: {item}")
            self.q.put(item)
            # Simulate some delay
            time.sleep(0.2)
        # Put a sentinel (None) to tell consumer to stop
        self.q.put(None)

    def consume(self):
        # Continuously read from the queue
        # Use sentinel pattern. If None is placed into the queue, that signals that there are no more
        # items to process and you can exit the loop.
        while True:
            item = self.q.get()
            if item is None:  # if sentinel is received, break
                break
            print(f"Consuming: {item}")
            self.q.task_done()

    def run(self, items):
        # Start the consumer thread in daemon mode so it quits with the main thread
        consumer_thread = threading.Thread(target=self.consume, daemon=True)
        consumer_thread.start()
        # Produce items
        self.produce(items)
        # Wait for all tasks in the queue to be marked done
        self.q.join()


def producer_consumer_example():
    logs = [f"Log Event #{i}" for i in range(1, 6)]
    pc = ProducerConsumer()
    pc.run(logs)


if __name__ == '__main__':
    producer_consumer_example()
```

## Web Crawler (Queue system)

An oversimplified web crawler.

```python
import requests
from collections import deque
from urllib.parse import urljoin, urlparse
import re


class SynchronousCrawler:
    def __init__(self, start_url, max_depth=2):
        self.start_url = start_url
        self.max_depth = max_depth
        self.visited = set()

    def is_same_domain(self, url1, url2):
        return urlparse(url1).netloc == urlparse(url2).netloc

    def extract_links(self, current_url, html):
        pattern = r'href="([^"]+)"'
        for match in re.findall(pattern, html, re.IGNORECASE):
            try:
                yield urljoin(current_url, match)
            # To prevent a strange URL parsing error.
            except ValueError:
                pass

    def crawl(self):
        # Double ended queue - allows us to remove visited URLs from the left 
        # and add new URLs to the right. 
        queue = deque([(self.start_url, 0)])

        while queue:
            # Remove from the left
            current_url, depth = queue.popleft()
            # membership check
            # Oversimplified - matching URLs only
            if current_url in self.visited:
                continue
            self.visited.add(current_url)
            # depth check
            if depth > self.max_depth:
                continue

            try:
                print(f"Crawling: {current_url} (depth {depth})")
                resp = requests.get(current_url, timeout=5)
                resp.raise_for_status()
                # Oversimplified: extract links with a regex pattern.
                for link in self.extract_links(current_url, resp.text):
                    # Only append to the queue if it's unique, unvisited, and same domain
                    if link not in self.visited and self.is_same_domain(self.start_url, link):
                        queue.append((link, depth + 1))

            except requests.RequestException as e:
                print(f"Error crawling {current_url}: {e}")

        return self.visited


if __name__ == "__main__":
    crawler = SynchronousCrawler("https://www.google.com", max_depth=1)
    results = crawler.crawl()
    print(f"\nCrawled {len(results)} URLs.")

```

Other considerations:

* Rate limiting
* Supplying seed URLs
* Authentication
* Executing sequences or filling out a form
* Excluding URLs or regex pattern
* Other variations (this one just makes HTTP GET requests and analyzes the HTML; running Selenium could let us use the DOM, execute JavaScript, access LocalStorage/Session Storage/IndexedDB.
