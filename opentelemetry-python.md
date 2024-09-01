# OpenTelemetry (Python)

Automatic instrumentation with OpenTelmetry is great, but OpenTelemetry becomes a total pain in the ass when you have to do manual instrumentation. In my opinion, the library exposes way too many of the guts of how OpenTelemetry works, rather than a developer experience of [Progressive Disclosure](https://www.uxpin.com/studio/blog/what-is-progressive-disclosure/).

Here are some helper methods that I created to make things easier. I found myself copying and pasting this a lot. Storing this on my cheatsheet site for easy reference.

I've used Python type hints where possible to make it easier to understand what value types are being returned by the various functions.

## Context Kung Fu

Managing the otel context object is one of the most frustrating points with manual instrumentation, in my short experience with OpenTelemetry. A close second is formatting the trace ID and span ID.&#x20;

### Get a span ID and trace ID strings inside a (Python) context manager (pun not intended)

The `get_span_context` method returns a SpanContext object, which supplies `span_id` and `trace_id` as integers, not strings. To convert it to strings (more useful) you should use the `format_span_id` and `format_trace_id` methods.

```python
from opentelemetry.context.context import Context
from opentelemetry.trace import format_span_id, format_trace_id, SpanContext, NonRecordingSpan

with tracer.start_as_current_span("my_span") as span:
    span_context: SpanContext = span.get_span_context()
    span_id: str = format_span_id(span_context.span_id)
    trace_id: str = format_trace_id(span_context.trace_id)
    # You can get the otel_context object from here too
    otel_context: Context = trace.set_span_in_context(NonRecordingSpan(span_context))

```

### Get the parent context object, given a trace ID and span ID

I want to be able to get a context object by supplying a trace ID string and span ID string.

<pre class="language-python"><code class="lang-python">from opentelemetry.context.context import Context
from opentelemetry.trace import NonRecordingSpan, SpanContext, TraceFlags

<strong>def get_parent_context(trace_id, span_id) -> Context:
</strong>    if isinstance(trace_id, str):
        trace_id = int(trace_id, 16)
    if isinstance(span_id, str):
        span_id = int(span_id, 16)
    parent_context = SpanContext(
        trace_id=trace_id,
        span_id=span_id,
        is_remote=True,
        trace_flags=TraceFlags(0x01)
    )
    return trace.set_span_in_context(NonRecordingSpan(parent_context))
    
# Usage
ctx = get_parent_context(trace_id, span_id)
</code></pre>

### Get the current span from anywhere and set an attribute

I want to get the current span context and set an attribute, knowing that I am within some span. That way I don't need to start a new span if I'm inside a child function.

```python
from opentelemetry.trace.span import Span

span: Span = trace.get_current_span()
span.set_attribute("foo", bar)
```

&#x20;

## Parse traceparent header

```python
def get_trace_id_from_traceparent(traceparent):
    parts = traceparent.split('-')
    if len(parts) < 2:
        return None  # Invalid traceparent header
    return parts[1]


def get_span_id_from_traceparent(traceparent):
    parts = traceparent.split('-')
    if len(parts) < 2:
        return None  # Invalid traceparent header
    return parts[2]
    
# Usage
trace_id = get_trace_id_from_traceparent(traceparent)
span_id = get_span_id_from_traceparent(traceparent)

```

## Start a span without a Python context manager

Note for readers: Context and SpanContext objects are different than Python context managers.

```python
from opentelemetry.context.context import Context
from opentelemetry import trace
from opentelemetry.trace import NonRecordingSpan, Span

tracer = trace.get_tracer(__name__)

# Start the span
parent_span: Span = tracer.start_span("parent")

# Get the context object so we can pass it to child functions
otel_context: Context = trace.set_span_in_context(NonRecordingSpan(parent_span.get_span_context()))

# Run child functions and pass otel_context object to it. You can use this for nested spans inside those functions.
child_function(otel_context)

# End the span
parent_span.end()
```

## Events

### Add events with attributes

The Instrumentation page shows how to set events, but you can also set attributes for specific events to prevent your event name from going wild.

```python
from opentelemetry import trace

current_span = trace.get_current_span()
command_str = "my long command string"

# Example: Event Attributes to store extra information
current_span.add_event("Service.Purpose.RunCommand", attributes={"command": command_str})

# Run your process
response = run_command(command_str)

# Store the result
current_span.add_event("Did it!", attributes={"response": response})
```

## Exceptions and Errors

### Raise a soft exception

```
from opentelemetry.trace import Status, StatusCode

class SomeKindOfError(Exception):
    pass

# Within a span
message = "Insert a message here"
span.set_status(Status(StatusCode.ERROR))
span.record_exception(SomeKindofError(message), attributes={"extra_info": "here"})
```

## Filtering out Span Events based on Attributes

* Working on this, but for boto3 calls: [Filtering Span Events and Other Data](https://docs.honeycomb.io/send-data/opentelemetry/collector/#filtering-span-events-and-other-data)

## Fake values

### Placeholder Trace ID and span ID

```
trace_id = "00000000000000000000000000000000"
span_id = "0000000000000000"
```

## References

* The [Cookbook page](https://opentelemetry.io/docs/languages/python/cookbook/) on the Python library documentation site is great, but the [Instrumentation page](https://opentelemetry.io/docs/languages/python/instrumentation/) has a lot more code examples.
* The AWS OpenTelemetry documentation has some good code examples [here](https://aws-otel.github.io/docs/getting-started/python-sdk/manual-instr#custom-instrumentation).
