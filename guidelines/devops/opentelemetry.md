# OpenTelemetry Guidelines

## Architecture Overview

```text
application/
├── instrumentation/     # Instrumentation configuration
│   ├── tracer.ts       # Tracer setup
│   ├── meter.ts        # Metrics setup
│   └── logger.ts       # Logging setup
├── exporters/          # Data exporters
└── processors/         # Signal processors

collector/
├── config.yaml         # Collector configuration
└── pipelines/         # Data processing pipelines
```

## Core Principles

- Context propagation
- Consistent instrumentation
- Sampling strategy
- Resource attribution
- Signal correlation

## Signal Types

- Traces
  * Span context
  * Attributes
  * Events
  * Links
- Metrics
  * Counters
  * Gauges
  * Histograms
- Logs
  * Structured logging
  * Severity levels
  * Context injection

## Quality Standards

- Semantic conventions
- Attribute naming
- Sampling configuration
- Correlation standards
- Performance impact monitoring

## Code Examples

### Tracer Setup

```typescript
import * as opentelemetry from '@opentelemetry/api';
import { Resource } from '@opentelemetry/resources';
import { SemanticResourceAttributes } from '@opentelemetry/semantic-conventions';

const resource = new Resource({
  [SemanticResourceAttributes.SERVICE_NAME]: 'my-service',
  [SemanticResourceAttributes.SERVICE_VERSION]: '1.0.0',
  environment: 'production'
});

const tracerProvider = new NodeTracerProvider({
  resource,
  sampler: new ParentBasedSampler({
    root: new TraceIdRatioBased(0.1)
  })
});

tracerProvider.addSpanProcessor(
  new BatchSpanProcessor(new OTLPTraceExporter())
);

tracerProvider.register();

const tracer = opentelemetry.trace.getTracer('my-service');
```

### Metrics Configuration

```typescript
import { MeterProvider } from '@opentelemetry/metrics';

const meter = new MeterProvider({
  resource: resource
}).getMeter('my-service');

const requestCounter = meter.createCounter('http.requests', {
  description: 'Count of HTTP requests',
  unit: '1'
});

const latencyHistogram = meter.createHistogram('http.latency', {
  description: 'HTTP request latency',
  unit: 'ms'
});
```

## Collector Configuration

```yaml
receivers:
  otlp:
    protocols:
      grpc:
        endpoint: 0.0.0.0:4317
      http:
        endpoint: 0.0.0.0:4318

processors:
  batch:
    timeout: 1s
    send_batch_size: 1024
  
  memory_limiter:
    check_interval: 1s
    limit_mib: 1024

exporters:
  prometheus:
    endpoint: 0.0.0.0:8889
  
  elasticsearch:
    endpoints: ["http://elasticsearch:9200"]

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [memory_limiter, batch]
      exporters: [elasticsearch]
    
    metrics:
      receivers: [otlp]
      processors: [memory_limiter, batch]
      exporters: [prometheus]
```

## Instrumentation Standards

- Auto-instrumentation where possible
- Manual instrumentation for custom code
- Consistent attribute naming
- Context propagation
- Error tracking

## Security Standards

- TLS encryption
- Authentication
- Authorization
- PII handling
- Data scrubbing

## Best Practices

- Effective sampling
- Attribute filtering
- Resource detection
- Batch processing
- Load balancing

## Performance Guidelines

- Sampling optimization
- Buffer sizes
- Batch configuration
- Export intervals
- Resource limits

## Error Handling

```typescript
import { SpanStatusCode } from '@opentelemetry/api';

async function handleRequest(ctx: Context) {
  const span = tracer.startSpan('handleRequest');
  
  try {
    const result = await processRequest(ctx);
    span.setStatus({ code: SpanStatusCode.OK });
    return result;
  } catch (error) {
    span.setStatus({
      code: SpanStatusCode.ERROR,
      message: error.message
    });
    span.recordException(error);
    throw error;
  } finally {
    span.end();
  }
}
```

## Integration Patterns

- Service mesh integration
- Load balancer visibility
- Database monitoring
- Cache monitoring
- Queue tracking

## Data Pipeline Design

```mermaid
flowchart LR
    App[Application] --> Collector[OpenTelemetry Collector]
    Collector --> Processors[Signal Processors]
    Processors --> Exporters[Exporters]
    Exporters --> Backend[Backend Systems]
