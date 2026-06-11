---
title: 'Building Production-Grade Kubernetes Operators with Java'
date: 2026-06-20
description: 'Building production-grade Kubernetes Operators using Java Operator SDK (JOSDK) and Fabric8 Kubernetes Client'
event: 'FOSS United Pune'
tags: ['kubernetes', 'java', 'josdk', 'operators', 'fabric8']
theme: black
fonts:
  - 'Inter:wght@400;500;600;700'
  - 'Space Grotesk:wght@400;700'
  - 'JetBrains Mono:wght@400;700'
footer:
  left: 'ashish thakur · red hat'
  right: 'foss united pune · june 2026'
customCSS: |
  .reveal .slides { text-align: left; }
  .reveal .controls { color: #3b82f6; }
  .reveal .progress { color: #3b82f6; height: 3px; }
revealOptions: |
  transition: 'slide',
  controlsTutorial: false,
  width: 1280,
  height: 720,
  margin: 0.02,
  minScale: 0.2,
  maxScale: 2.0,
---

<section class="title-slide">

# Building Production-Grade<br>*Kubernetes Operators*<br>with Java

<div class="author-info">
<strong>Ashish Thakur</strong><br>
Senior Software Engineer · Red Hat
</div>

</section>

<section class="content-slide about-slide">

## About Me

<div class="cards">
<div class="card">
<h3>Ashish Thakur</h3>
<p><strong style="color:#e2e8f0">Senior Software Engineer</strong> at <strong style="color:#e2e8f0">Red Hat</strong></p>
<p>Contributor to <strong style="color:#60a5fa">Fabric8 Kubernetes Client</strong> — the industry-standard Java client for Kubernetes & OpenShift</p>
<p style="margin-top:.4em">
<span class="stat">3.7k stars</span>
<span class="stat">132 releases</span>
<span class="stat">11 years</span>
</p>
</div>
<div class="card">
<h3>Connect</h3>
<ul>
<li>github.com/ash-thakur-rh</li>
<li>ashthakur.in/blog</li>
</ul>
<p style="margin-top:.4em;font-size:.85em;color:#94a3b8">Part of the CNCF Operator Framework ecosystem</p>
</div>
</div>

</section>

<section class="content-slide">

## The Problem

<div class="problem-grid">
<div class="card" style="border-left:3px solid #22c55e">
<h3>Day 1 is Easy</h3>
<p><code style="color:#22d3ee">kubectl apply -f deployment.yaml</code></p>
<p>Deploy your app. It works. Ship it.</p>
</div>
<div class="card" style="border-left:3px solid #ef4444">
<h3>Day 2 is Hard</h3>
<ul>
<li>Upgrades & zero-downtime rollouts</li>
<li>Scaling based on custom metrics</li>
<li>Backup, restore, failover</li>
<li>Configuration drift & recovery</li>
</ul>
</div>
</div>

<div class="fragment" style="margin-top:.6em;text-align:center">
<p style="font-size:.75em;color:#94a3b8">Human operators can't be on-call 24/7 →</p>
<p style="font-size:.85em;color:#22d3ee;font-weight:600">Encode operational knowledge into software</p>
</div>

</section>

<section class="content-slide">

## What are Kubernetes Operators?

<div class="cards" style="margin-top:.2em">
<div class="card">
<h3>Definition</h3>
<p>Software extensions that manage cluster and non-cluster resources on behalf of Kubernetes.</p>
</div>
<div class="card">
<h3>The Goal</h3>
<p>Automate complex Day 2 operations — upgrades, failover, scaling — via custom controllers.</p>
</div>
</div>

<div class="flow" style="margin-top:.6em">
<div class="flow-step">
<h3>Custom Resource</h3>
<p>"Desired State" from the user</p>
</div>
<div class="flow-arrow">→</div>
<div class="flow-step">
<h3>Controller</h3>
<p>Watches for changes</p>
</div>
<div class="flow-arrow">→</div>
<div class="flow-step">
<h3>Reconciliation</h3>
<p>Current → Desired state</p>
</div>
</div>

</section>

<section class="content-slide">

## Operator vs. Controller

<div class="cards">
<div class="card">
<h3>Controller</h3>
<p>A generic control loop that watches built-in K8s resources (Pods, Services) and reacts.</p>
<p>Uses <strong style="color:#60a5fa">Fabric8 Kubernetes Client</strong> directly.</p>
</div>
<div class="card">
<h3>Operator</h3>
<p>A specialized Controller that uses <strong style="color:#22d3ee">Custom Resource Definitions (CRDs)</strong> to extend the K8s API.</p>
<p>A Controller with <em>domain knowledge</em>.</p>
</div>
</div>

<div style="margin-top:.6em;text-align:center">
<p style="font-size:.65em;color:#94a3b8">Every Operator is a Controller, but not every Controller is an Operator.</p>
<p style="font-size:.6em;margin-top:.3em">
<span style="color:#60a5fa;font-weight:600">Fabric8 Client</span>
<span style="color:#94a3b8"> → Foundation → </span>
<span style="color:#22d3ee;font-weight:700">JOSDK</span>
<span style="color:#94a3b8"> → High-level Operator Framework</span>
</p>
</div>

</section>

<section class="content-slide">

## Real-World Example

<div class="cards">
<div class="card" style="border-left:3px solid #ef4444">
<h3>Without an Operator</h3>
<ul>
<li>Manually create Deployment, Service, ConfigMap</li>
<li>Manually create Secret with DB credentials</li>
<li>Manually deploy PostgreSQL StatefulSet</li>
<li>Configure Ingress, HPA separately</li>
<li><strong style="color:#ef4444">6+ YAML files, error-prone</strong></li>
</ul>
</div>
<div class="card" style="border-left:3px solid #22c55e">
<h3>With an Operator</h3>

```yaml
apiVersion: example.io/v1
kind: MicroService
metadata:
  name: petclinic
spec:
  image: spring-petclinic:3.5.6
  replicas: 2
  database: { databaseName: petclinicdb }
  exposed: true
```

<p><strong style="color:#22c55e">One resource. Operator handles the rest.</strong></p>
</div>
</div>

</section>

<section class="content-slide">

## The Java Stack

<div style="text-align:center;margin-top:.2em">
<p style="font-size:.7em;color:#94a3b8;margin-bottom:.5em">Why Java? Massive enterprise adoption. Existing teams. Mature ecosystem.</p>
</div>

<div class="stack-diagram">
<div class="stack-layer stack-top">
<div class="stack-label">Your Operator</div>
<div class="stack-desc">Business logic & domain knowledge</div>
</div>
<div class="stack-layer stack-mid">
<div class="stack-label">Java Operator SDK (JOSDK)</div>
<div class="stack-desc">Reconciliation, events, retries, dependent resources</div>
</div>
<div class="stack-layer stack-bottom">
<div class="stack-label">Fabric8 Kubernetes Client</div>
<div class="stack-desc">Fluent API, CRUD, watch, CRDs, OpenShift support</div>
</div>
<div class="stack-layer stack-base">
<div class="stack-label">Kubernetes API</div>
<div class="stack-desc">REST API & etcd</div>
</div>
</div>

</section>

<section class="content-slide">

## Fabric8 Kubernetes Client

<p style="font-size:.65em;color:#94a3b8;margin-bottom:.3em">The industry-standard Java client for Kubernetes & OpenShift</p>

<div class="feature-grid" style="grid-template-columns:repeat(3,1fr)">
<div class="feat">
<h3>Fluent DSL</h3>
<p>Chainable, readable API</p>
</div>
<div class="feat">
<h3>OpenShift First-Class</h3>
<p>Full support, unlike official client</p>
</div>
<div class="feat">
<h3>CRD Support</h3>
<p>Code gen + Maven plugin</p>
</div>
<div class="feat">
<h3>Built-in Testing</h3>
<p>Mock server, CRUD mode</p>
</div>
<div class="feat">
<h3>Extensions</h3>
<p>Knative, Tekton, Istio</p>
</div>
<div class="feat">
<h3>Massive Adoption</h3>
<p>Flink, Spark, Jenkins, Strimzi</p>
</div>
</div>

</section>

<section class="content-slide">

## Fabric8 in Action

<p style="font-size:.6em;color:#94a3b8;margin-bottom:.2em">Fluent DSL for CRUD, watching, and custom resources</p>

```java
// List pods by label
client.pods().inNamespace("production")
    .withLabel("app", "petclinic").list();

// Create a resource
client.resource(deployment).create();

// Watch for real-time events
client.pods().watch(new Watcher<Pod>() {
    public void eventReceived(Action action, Pod pod) {
        log.info("{}: {}", action, pod.getMetadata().getName());
    }
});

// Type-safe Custom Resources
client.resources(MicroService.class)
    .inNamespace("production").withName("petclinic").get();
```

</section>

<section class="content-slide">

## From Client to Framework

<p style="font-size:.65em;color:#94a3b8;margin-bottom:.3em">Fabric8 is the client. Building a production operator needs more:</p>

<div class="gap-list">
<div class="gap-item fragment">
<span class="gap-icon">↻</span>
<div><strong>Reconciliation Loop</strong><span>Watch → compare → act cycle with retry logic</span></div>
</div>
<div class="gap-item fragment">
<span class="gap-icon">⚡</span>
<div><strong>Event Handling</strong><span>Deduplication, batching, scheduling</span></div>
</div>
<div class="gap-item fragment">
<span class="gap-icon">🔗</span>
<div><strong>Dependent Resources</strong><span>Declarative secondary resource management</span></div>
</div>
<div class="gap-item fragment">
<span class="gap-icon">📊</span>
<div><strong>Observability</strong><span>Health checks, metrics, structured logging</span></div>
</div>
</div>

<p class="fragment" style="font-size:.75em;color:#22d3ee;margin-top:.4em;text-align:center;font-weight:600">→ Java Operator SDK (JOSDK)</p>

</section>

<section class="content-slide">

## JOSDK — Key Concepts

<div class="cards" style="gap:10px">
<div class="card">
<h3>Reconciler</h3>
<p>Implement <code style="color:#22d3ee">reconcile()</code> — framework handles the loop, retries, and error recovery.</p>
</div>
<div class="card">
<h3>Dependent Resources</h3>
<p>Declaratively manage secondary resources. Auto-creates, updates, garbage-collects.</p>
</div>
<div class="card">
<h3>Event Sources</h3>
<p>Custom triggers beyond K8s API — external systems, timers, conditions.</p>
</div>
</div>

<p style="font-size:.6em;color:#94a3b8;margin-top:.5em;text-align:center">
<span class="stat">CNCF Project</span>
<span class="stat">929 stars</span>
<span class="stat">Quarkus + Spring Boot</span>
</p>

</section>

<section class="content-slide">

## Define Your Custom Resource

<div class="cards">
<div class="card">
<h3>Java CRD Class</h3>

```java
@Group("example.io")
@Version("v1")
@ShortNames("ms")
public class MicroService extends
  CustomResource<MicroServiceSpec,
                 MicroServiceStatus>
  implements Namespaced { }
```

</div>
<div class="card">
<h3>What Users Write</h3>

```yaml
apiVersion: example.io/v1
kind: MicroService
metadata:
  name: petclinic
spec:
  image: spring-petclinic:3.5.6
  replicas: 2
  exposed: true
```

</div>
</div>

<p style="font-size:.6em;color:#94a3b8;margin-top:.3em;text-align:center">One simple resource — the operator creates Deployments, Services, ConfigMaps, database, HPA, Ingress automatically.</p>

</section>

<section class="content-slide">

## Full Custom Resource — Production Example

<p style="font-size:.6em;color:#94a3b8;margin-bottom:.2em">Operator-managed database, autoscaling, ingress — all from one CR</p>

```yaml
apiVersion: example.io/v1
kind: MicroService
metadata:
  name: petclinic-prod
  namespace: production
spec:
  image: quay.io/ash-thakur/spring-petclinic:4.0.3-arm64
  replicas: 2
  containerPort: 8080
  database:                              # Operator provisions PostgreSQL
    databaseName: petclinicdb
    username: petclinic
    storageSize: 5Gi
  autoscaling: { minReplicas: 2, maxReplicas: 8, targetCPUUtilizationPercentage: 20 }
  exposed: true
  hostname: petclinic.apps.mycluster.example.com
```

<p style="font-size:.55em;color:#22d3ee;margin-top:.2em;text-align:center">Auto-generates DB credentials Secret, StatefulSet, headless Service — <strong>no manual kubectl create secret.</strong></p>

</section>

<section class="content-slide">

## Implementing the Reconciler

<p style="font-size:.6em;color:#94a3b8;margin-bottom:.2em">@Workflow declares the dependency graph — reconcile() just reads status</p>

```java
@Workflow(dependents = {
    @Dependent(type = ConfigMapDependentResource.class),
    @Dependent(type = DeploymentDependentResource.class,
               dependsOn = "ConfigMap"),
    @Dependent(type = ServiceDependentResource.class,
               dependsOn = "Deployment"),
    @Dependent(type = IngressDependentResource.class,
               reconcilePrecondition = ExposedCondition.class)
})
@ControllerConfiguration
public class MicroServiceReconciler
    implements Reconciler<MicroService> {

    public UpdateControl<MicroService> reconcile(
            MicroService ms, Context<MicroService> ctx) {
        int ready = ctx.getSecondaryResource(Deployment.class)
            .map(d -> d.getStatus().getReadyReplicas())
            .orElse(0);
        return UpdateControl.patchStatus(
            buildStatusPatch(ms, ready));
    }
}
```

</section>

<section class="content-slide">

## Dependent Resources

<p style="font-size:.6em;color:#94a3b8;margin-bottom:.2em">Define desired state — the framework diffs against reality and reconciles</p>

```java
public class DeploymentDependentResource
    extends CRUDKubernetesDependentResource
             <Deployment, MicroService> {

    protected Deployment desired(MicroService ms,
                        Context<MicroService> ctx) {
        return new DeploymentBuilder()
            .withNewMetadata()
                .withName(ms.getMetadata().getName())
                .withNamespace(ms.getMetadata().getNamespace())
            .endMetadata()
            .withNewSpec()
                .withReplicas(ms.getSpec().getReplicas())
                .withNewSelector()
                  .withMatchLabels(Map.of("app",
                    ms.getMetadata().getName()))
                .endSelector()
                // ... template, container, probes ...
            .endSpec().build();
    }
}
```

</section>

<section class="content-slide">

## Patterns and Best Practices

<div class="practices">
<div class="practice">
<strong>Idempotency is Key</strong>
<span>— Reconcile must be safe to run multiple times with the same result.</span>
</div>
<div class="practice">
<strong>Always Reconcile All Resources</strong>
<span>— Check actual state of all components, not just the triggering event.</span>
</div>
<div class="practice">
<strong>Stateless Logic</strong>
<span>— Use K8s Status sub-resources to track progress, not in-memory state.</span>
</div>
<div class="practice">
<strong>Leverage Caching</strong>
<span>— Use Informers and Event Sources to avoid excessive API calls.</span>
</div>
</div>

</section>

<section class="content-slide">

## Error Handling

<p style="font-size:.6em;color:#94a3b8;margin-bottom:.2em">Write errors to status subresource — cleanup uses owner references for automatic GC</p>

```java
@Override
public ErrorStatusUpdateControl<MicroService> updateErrorStatus(
        MicroService ms, Context<MicroService> ctx, Exception e) {
    log.error("Error reconciling {}/{}: {}",
        ms.getMetadata().getNamespace(),
        ms.getMetadata().getName(), e.getMessage());
    MicroService patch = buildStatusPatch(ms, Phase.ERROR, 0);
    patch.getStatus()
         .setMessage("Reconciliation error: " + e.getMessage());
    return ErrorStatusUpdateControl.patchStatus(patch);
}

@Override
public DeleteControl cleanup(MicroService ms,
                              Context<MicroService> ctx) {
    // Owner references handle garbage collection automatically
    return DeleteControl.defaultDelete();
}
```

</section>

<section class="content-slide">

## Who's Using It in Production?

<div class="adopters-grid">
<div class="adopter">
<h3>Keycloak Operator</h3>
<p>Official, Quarkus + JOSDK</p>
</div>
<div class="adopter">
<h3>Apache Flink</h3>
<p>K8s Operator — "market leader"</p>
</div>
<div class="adopter">
<h3>Apache Spark</h3>
<p>K8s Operator for Spark</p>
</div>
<div class="adopter">
<h3>Strimzi</h3>
<p>Kafka on Kubernetes</p>
</div>
<div class="adopter">
<h3>Debezium</h3>
<p>Change Data Capture</p>
</div>
<div class="adopter">
<h3>Glasskube</h3>
<p>K8s package manager</p>
</div>
</div>

</section>

<section class="content-slide resources">

## Resources

- **Fabric8 Client**: [github.com/fabric8io/kubernetes-client](https://github.com/fabric8io/kubernetes-client)
- **JOSDK**: [javaoperatorsdk.io](https://javaoperatorsdk.io) · [GitHub](https://github.com/operator-framework/java-operator-sdk)
- **Demo project**: [github.com/ash-thakur-rh/microservice-operator](https://github.com/ash-thakur-rh/microservice-operator)
- **Blog**: [ashthakur.in/blog](https://www.ashthakur.in/blog)

</section>

<section class="divider">

## DEMO

</section>

<section class="thanks-slide">

## Thanks!

<div class="contact">
<strong>Ashish Thakur</strong><br>
Senior Software Engineer · Red Hat
</div>

<a class="github-link" href="https://github.com/ash-thakur-rh">github.com/ash-thakur-rh</a>

</section>
