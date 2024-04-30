# OpenTelemetry (Python)

Automatic instrumentation with OpenTelmetry is great, but OpenTelemetry becomes a total pain in the ass when you have to do manual instrumentation. In my opinion, the library exposes way too many of the guts of how OpenTelemetry works, rather than a developer experience of [Progressive Disclosure](https://www.uxpin.com/studio/blog/what-is-progressive-disclosure/).

Here are some helper methods that I created to make things easier. I found myself copying and pasting this a lot. Storing this on my cheatsheet site for easy reference.

## Context Kung Fu

Managing the otel context object is one of the most frustrating points with manual instrumentation, in my short experience with OpenTelemetry. A close second is formatting the trace ID and span ID.&#x20;

### Get a span ID and trace ID strings inside a context manager

The `get_span_context` method returns a SpanContext object, which supplies `span_id` and `trace_id` as integers, not strings. To convert it to strings (more useful) you should use the `format_span_id` and `format_trace_id` methods.

```python
from opentelemetry.trace import format_span_id, format_trace_id

with tracer.start_as_current_span("my_span") as span:
    span_context = span.get_span_context()
    span_id = format_span_id(span_context.span_id)
    trace_id = format_trace_id(span_context.trace_id)

```

### Get the parent context object, given a trace ID and span ID

I want to be able to get a context object by supplying a trace ID string and span ID string.

<pre class="language-python"><code class="lang-python">from opentelemetry.context.context import Context

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

# Start the span
parent_span = tracer.start_span("parent")

# Get the context object so we can pass it to child functions
otel_context: Context = trace.set_span_in_context(NonRecordingSpan(parent_span.get_span_context()))

# Run child functions and pass otel_context object to it. You can use this for nested spans inside those functions.
child_function(otel_context)

# End the span
parent_span.end()
```
