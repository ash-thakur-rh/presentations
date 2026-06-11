# Talk Script: Building Production-Grade Kubernetes Operators with Java

**Event:** FOSS United Pune · June 20, 2026
**Duration:** ~30-35 minutes + demo + Q&A
**Speaker:** Ashish Thakur, Senior Software Engineer, Red Hat

---

## Talk Arc

```
WHY → WHAT → HOW → BUILD → HARDEN → PROVE → DEMO
```

**Act 1 (slides 1-6):** Set the context — who you are, the problem, what operators are, why they matter.
**Act 2 (slides 7-11):** Introduce the Java tooling — Fabric8 as the foundation, JOSDK as the framework.
**Act 3 (slides 12-17):** Build it — CRD, reconciler, dependent resources, best practices, error handling.
**Act 4 (slides 18-21):** Social proof, resources, live demo, close.

---

## Slide-by-Slide Script

### Slide 1 — Title

> Hi everyone, I'm Ashish Thakur. Today we're going to talk about building production-grade Kubernetes Operators — but we're going to do it in Java, not Go.
>
> By the end of this talk, you'll understand what operators are, why Java is a great choice for building them, and you'll see real code from an open-source operator I've built.

**Transition:** "But first, a quick intro."

---

### Slide 2 — About Me

> I'm a Senior Software Engineer at Red Hat. I contribute to the Fabric8 Kubernetes Client — it's the most widely used Java client for Kubernetes. 3.7k stars, 132 releases, 11 years of history. If you've used any Java tool that talks to Kubernetes — Jenkins, Apache Flink, Strimzi, Spring Cloud Kubernetes — chances are it uses Fabric8 under the hood.
>
> I'm also part of the CNCF Operator Framework ecosystem, which is how I got into building operators with Java.

**Transition:** "So why do we even need operators? Let me paint a picture."

---

### Slide 3 — The Problem

> Day 1 with Kubernetes is great. You write your YAML, run `kubectl apply`, and your app is running. Easy.
>
> But then comes Day 2. You need to do a zero-downtime upgrade. You need to scale based on custom business metrics, not just CPU. Your database needs backup and failover. Configuration drifts happen in production.
>
> *(fragment appears)*
>
> And here's the thing — humans can't be on-call 24/7 watching every resource. We need to encode that operational knowledge into software that runs inside the cluster, watching and reacting continuously. That's exactly what operators do.

**Transition:** "So what exactly is a Kubernetes Operator?"

---

### Slide 4 — What are Kubernetes Operators?

> An operator is a software extension that manages resources on behalf of Kubernetes. The goal is simple: automate the complex Day 2 operations that humans would otherwise do manually.
>
> The pattern has three parts. First, the user creates a Custom Resource — that's the desired state. A controller watches for changes to that resource. And the reconciliation loop continuously works to make the current state match the desired state.
>
> This is the heart of the operator pattern — it's a never-ending loop. Something breaks? The operator notices and fixes it. Someone deletes a pod? The operator recreates it. Configuration drifts? The operator corrects it.

**Transition:** "Now there's an important distinction here."

---

### Slide 5 — Operator vs. Controller

> A controller is a generic loop — it watches built-in Kubernetes resources like Pods and Services and reacts. You can build a controller with just the Fabric8 Client.
>
> An operator is a specialized controller. The key difference? It uses Custom Resource Definitions — CRDs — to extend the Kubernetes API. It's a controller with domain knowledge. It understands your application, your database, your deployment strategy.
>
> Every operator is a controller, but not every controller is an operator. And our journey today goes from Fabric8 as the foundation to JOSDK as the high-level operator framework.

**Transition:** "Let me show you why this matters with a concrete example."

---

### Slide 6 — Real-World Example

> Imagine you want to deploy a microservice with a PostgreSQL database.
>
> Without an operator, you're manually creating a Deployment, a Service, a ConfigMap, a Secret with database credentials, a PostgreSQL StatefulSet, an Ingress, maybe an HPA. That's 6+ YAML files, hundreds of lines, and it's error-prone. Forget one label selector and things break silently.
>
> With an operator? You write one Custom Resource — 10 lines of YAML. The operator creates everything else. Database credentials are auto-generated. PostgreSQL is provisioned. Ingress is configured. Autoscaling is set up. One resource. The operator handles the rest.
>
> This is the power of the operator pattern. And this is the actual operator we'll be looking at today — the microservice-operator I built with JOSDK.

**Transition:** "So how do we build this in Java? Let me show you the stack."

---

### Slide 7 — The Java Stack

> Why Java? Because there are millions of Java developers in enterprises. They already have Java teams, Java CI/CD, Java expertise. Asking them to learn Go just to build an operator is a hard sell.
>
> Here's the stack. At the bottom is the Kubernetes API. On top of that sits the Fabric8 Kubernetes Client — that's the foundation. It gives you a beautiful fluent API for CRUD operations, watching resources, and working with CRDs.
>
> On top of Fabric8 sits the Java Operator SDK — JOSDK. It adds the reconciliation loop, event handling, dependent resource management, and all the production infrastructure you need.
>
> And on top of that sits your operator — your business logic and domain knowledge.

**Transition:** "Let's start with the foundation."

---

### Slide 8 — Fabric8 Kubernetes Client

> Fabric8 is the industry standard. Six key things make it stand out.
>
> First, the fluent DSL — every API call is chainable and readable. Second, first-class OpenShift support — the official Kubernetes Java client doesn't have this. Third, CRD support with code generation from schemas.
>
> Fourth, built-in testing with a mock server — you can test your code without a real cluster. Fifth, extensions for Knative, Tekton, Istio. And sixth, massive adoption — Apache Flink, Spark, Jenkins, Strimzi, Eclipse Che all use Fabric8.

**Transition:** "Let me show you what the code looks like."

---

### Slide 9 — Fabric8 in Action

> This is the fluent DSL in action. List pods by label — one line. Create a resource — one line. Watch for real-time events — implement a simple callback. Work with custom resources — completely type-safe.
>
> Notice how readable this is compared to raw HTTP calls or even the official Java client. This is why Fabric8 has been the standard for 11 years.

**Transition:** "But Fabric8 is a client library. Building a production operator needs more."

---

### Slide 10 — From Client to Framework

> *(fragments appear one by one)*
>
> You need a reconciliation loop — the continuous watch-compare-act cycle with retry logic. You need event handling — deduplication and batching so you don't reconcile 100 times for one change. You need dependent resource management — a way to declaratively manage all the secondary resources your operator creates. And you need observability — health checks, metrics, structured logging.
>
> *(final fragment)*
>
> This is exactly what the Java Operator SDK provides. It sits on top of Fabric8 and gives you the complete framework.

**Transition:** "Let's look at the key concepts."

---

### Slide 11 — JOSDK Key Concepts

> Three core abstractions.
>
> The Reconciler — implement one method, `reconcile()`, and the framework handles the loop, retries, and error recovery. You just write the logic.
>
> Dependent Resources — this is JOSDK's killer feature. You declaratively define the secondary resources your operator manages — Deployments, Services, ConfigMaps. The framework auto-creates, updates, and garbage-collects them.
>
> Event Sources — custom triggers beyond the Kubernetes API. You can watch external systems, timers, or custom conditions.
>
> JOSDK is a CNCF project, it has 929 stars, and it works with both Quarkus and Spring Boot.

**Transition:** "Let's build an operator. Step one — define your CRD."

---

### Slide 12 — Define Your Custom Resource

> On the left is the Java class. Three annotations — Group, Version, ShortNames. Extend CustomResource with your Spec and Status types. That's it.
>
> On the right is what your users write. Simple YAML. Image, replicas, exposed flag. The operator creates everything else — Deployment, Service, ConfigMap, Ingress.
>
> This is the contract between your users and your operator.

**Transition:** "Here's what a production-ready CR looks like."

---

### Slide 13 — Full Custom Resource — Production Example

> This is a real CR from our microservice-operator. Notice the `database` section — when you set this, the operator automatically provisions a PostgreSQL StatefulSet with a PVC, generates random credentials into a Secret, creates a headless Service, and injects the credentials into the app Deployment.
>
> No `kubectl create secret`. Ever. The operator handles the full lifecycle.
>
> You also see autoscaling and ingress configuration — all declarative, all managed by the operator.

**Transition:** "Now let's see the reconciler that makes this work."

---

### Slide 14 — Implementing the Reconciler

> This is the real reconciler from the microservice-operator. Look at the `@Workflow` annotation — it declares all the dependent resources and their dependency order. ConfigMap first, then Deployment depends on ConfigMap, Service depends on Deployment, and so on.
>
> The `reconcile()` method itself is tiny — it just reads the Deployment's ready replicas from the cache and patches the status. All the heavy lifting — creating Deployments, Services, ConfigMaps — is handled by the dependent resources.
>
> This is the beauty of JOSDK. Your reconciler stays clean and focused.

**Transition:** "Let me show you how dependent resources work."

---

### Slide 15 — Dependent Resources

> Each dependent resource is a class that manages one Kubernetes resource. You extend `CRUDKubernetesDependentResource` and implement one method — `desired()`.
>
> You return what the Deployment SHOULD look like. The framework compares it with what actually exists in the cluster and takes action — create if missing, update if different, leave alone if matching.
>
> Owner references are set automatically, so when you delete the custom resource, all dependent resources are garbage-collected. You don't write cleanup code.

**Transition:** "Now let's talk about doing this right in production."

---

### Slide 16 — Patterns and Best Practices

> Four critical patterns.
>
> Idempotency — your reconcile method will be called many times. It must produce the same result regardless of how many times it runs.
>
> Always reconcile all resources — don't just react to the event that triggered you. Check the actual state of everything you manage. An event might be stale or out of order.
>
> Stateless logic — don't keep state in memory. Use the Kubernetes Status subresource. If your operator restarts, it should pick up exactly where it left off by reading the cluster state.
>
> Leverage caching — don't hammer the API server. Use informers and event sources. They maintain a local cache that's kept in sync by the watch stream.

**Transition:** "And when things go wrong..."

---

### Slide 17 — Error Handling

> JOSDK gives you `updateErrorStatus` — when reconciliation fails, this method is called so you can write the error to the status subresource. Your users can see what went wrong by looking at the custom resource's status.
>
> For cleanup, you implement `cleanup()`. But notice it's one line — `defaultDelete()`. Because we set owner references on all dependent resources, Kubernetes garbage-collects them automatically when the custom resource is deleted.
>
> This is production-grade error handling with minimal code.

**Transition:** "You might be wondering — is anyone actually using this in production?"

---

### Slide 18 — Who's Using It in Production?

> Absolutely. The Keycloak Operator — the official one — is built with Quarkus and JOSDK. That's a major CNCF-adjacent identity platform.
>
> Apache Flink's Kubernetes Operator — described as the "market leader" among Flink operators — uses JOSDK. Apache Spark is building their operator with it too.
>
> Strimzi uses it for the Kafka ecosystem. Debezium for Change Data Capture. Glasskube for Kubernetes package management.
>
> This is not a toy framework. It's battle-tested at scale.

**Transition:** "Here are all the links you need."

---

### Slide 19 — Resources

> Everything is open source. The Fabric8 Client and JOSDK are both on GitHub. The JOSDK website has excellent documentation.
>
> The demo project — microservice-operator — is the code we've been looking at today. Clone it, run it with Kind or Minikube, and try it yourself.
>
> And my blog has more posts about operators and Kubernetes with Java.

**Transition:** "Now let me show you all of this working live."

---

### Slide 20 — DEMO

**Demo flow (5-7 minutes):**

1. **Show the project structure** — quick tour of the Maven project, CRD class, reconciler, dependent resources
2. **Start a Kind cluster** — `kind create cluster` (or have one pre-created)
3. **Build and run the operator locally** — `mvn clean install && mvn exec:java` (or Quarkus dev mode)
4. **Apply the CRD** — `kubectl apply -f target/classes/META-INF/fabric8/customresources.yml`
5. **Create a simple MicroService CR** — `kubectl apply -f k8s/microservice-sample.yaml`
6. **Watch resources being created** — `kubectl get pods,svc,configmap,deploy -w`
7. **Show the status** — `kubectl get microservice petclinic -o yaml` (show RUNNING phase, readyReplicas)
8. **Scale it** — edit replicas in the CR, watch the operator react
9. **Delete the CR** — show garbage collection cleaning up all resources
10. **Optional: show the production CR** with database provisioning

**Key talking points during demo:**
- "Notice I didn't create any Deployment or Service manually"
- "The operator detected the change and reconciled automatically"
- "All resources have owner references — delete the CR and everything cleans up"
- "The status subresource shows exactly what's happening"

---

### Slide 21 — Thanks

> Thank you! The code is all on GitHub. I'm happy to chat about Fabric8, JOSDK, or anything Kubernetes and Java.
>
> Questions?

---

## Timing Guide

| Section | Slides | Target Time |
|---------|--------|-------------|
| Intro & Problem | 1-6 | 8 min |
| Java Tooling | 7-11 | 8 min |
| Build It | 12-17 | 10 min |
| Proof & Close | 18-19 | 3 min |
| Demo | 20 | 5-7 min |
| Q&A | — | 5-10 min |
| **Total** | | **~35-45 min** |

## Tips

- **Before the talk:** Have the Kind cluster pre-created and the operator pre-built. Demo failures are the #1 confidence killer.
- **Slide 6 is the hook:** The Real-World Example is where the audience goes "oh, that's useful." Pause and let it sink in.
- **Slides 12-15 are dense:** Don't read the code line by line. Highlight the key annotations and the `desired()` method pattern. The audience can read the details later.
- **Demo fallback:** If the live demo fails, have a pre-recorded terminal session (asciinema) ready to play.
- **Q&A bait:** Mention "we chose JOSDK over Go's Operator SDK" early — someone will ask why during Q&A, and that's a great discussion.
