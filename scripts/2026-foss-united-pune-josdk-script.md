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
**Act 3 (slides 12-18):** Build it — CRD, CR, reconciler, dependent resources, best practices, error handling.
**Act 4 (slides 19-22):** Social proof, resources, live demo, close.

---

## Slide-by-Slide Script

### Slide 1 — Title

> Hi everyone, I'm Ashish Thakur. Today we're going to talk about building production-grade Kubernetes Operators — but we're going to do it in Java, not Go.
>
> By the end of this talk, you'll understand what operators are, why Java is a great choice for building them, and you'll see real code from an open-source operator I've built.

**Transition:** "But first, a quick intro."

---

### Slide 2 — About Me

> I'm a Senior Software Engineer at Red Hat. I contribute to the Fabric8 Kubernetes Client — it's the most widely used Java client for Kubernetes with over 1 million monthly downloads, 3.7k stars, and 11 years of history.
>
> Fabric8 is a building block for some major projects — Quarkus, Spring Cloud Kubernetes, Apache Flink — and it's also the foundation for JOSDK, the CNCF Operator Framework for Java, which is what we'll be talking about today.
>
> If you want to connect after the talk — my Twitter, LinkedIn, and GitHub are all on the slide. I also write about Kubernetes and Java on my blog.

**Transition:** "So why do we even need operators? Let me paint a picture."

---

### Slide 3 — The Problem

> Let's talk about the lifecycle of running software on Kubernetes.
>
> Day 0 is the planning phase — architecture decisions, tooling choices, capacity planning. This happens before you write any YAML.
>
> Day 1 is the fun part. You write your YAML, run `kubectl apply`, and your app is running. Easy. Ship it.
>
> But then comes Day 2. This is where reality hits. You need zero-downtime upgrades. You need scaling, backup, restore, failover. Configuration drifts happen in production and you need to detect and recover from them.
>
> *(fragment appears)*
>
> Humans can't be on-call 24/7 watching every resource. We need to encode that operational knowledge into software that runs inside the cluster, watching and reacting continuously. That's exactly what operators do.

**Transition:** "So what exactly is a Kubernetes Operator?"

---

### Slide 4 — What are Kubernetes Operators?

> An operator is a software extension that manages resources on behalf of Kubernetes. The goal is simple: automate the complex Day 2 operations that humans would otherwise do manually.
>
> The pattern has three steps. First — Declare. The user specifies *what* they want by creating a Custom Resource. Then — Watch. A controller detects that desired state change. And finally — Reconcile. The controller continuously drives the current state toward the desired state.
>
> *(fragment appears)*
>
> This is fundamentally *declarative*. The user requests what they want, and the operator figures out how to make it happen. Something breaks? The operator notices and fixes it. Configuration drifts? The operator corrects it. This loop never stops.

**Transition:** "Now there's an important distinction here."

---

### Slide 5 — Operator vs. Controller

> A controller follows the Observe, Compare, Act pattern — it watches built-in Kubernetes resources like Pods and Services and reacts. No CRDs needed. Think of the built-in ReplicaSet, DaemonSet, and StatefulSet controllers.
>
> An operator is a specialized controller. The key difference? It uses Custom Resource Definitions — CRDs — to extend the Kubernetes API. It manages a single application's full lifecycle with domain knowledge. Think Strimzi for Kafka, the Prometheus Operator.
>
> Now, an important point — the Fabric8 Kubernetes Client can build *either* controllers or operators. It powers both. However, when building operators, JOSDK provides all the boilerplate and repetitive code you'd have to rewrite every time if you were using Fabric8 directly — the reconciliation loop, retries, event handling, dependent resource management.
>
> Every operator is a controller, but not every controller is an operator.

**Transition:** "Let me show you why this matters with a concrete example."

---

### Slide 6 — Real-World Example

> Imagine you want to deploy a microservice with a PostgreSQL database.
>
> On the left — without an operator — you manage everything yourself. Write a Deployment YAML, a Service YAML, a ConfigMap, create a Secret with database credentials, deploy a PostgreSQL StatefulSet, configure Ingress and HPA. That's 6+ YAML files, all manual, all error-prone. Forget one label selector and things break silently.
>
> On the right — with an operator — you declare your intent. One Custom Resource, about 8 lines of YAML. Image, replicas, database name, exposed flag. That's it.
>
> But here's the key — those Deployments, Services, ConfigMaps, Secrets, Ingress resources? They still exist. The operator auto-creates and manages all of them behind the scenes. The user just doesn't have to deal with them directly anymore.
>
> This is the actual operator we'll be looking at today — the microservice-operator I built with JOSDK.

**Transition:** "So how do we build this in Java? Let me show you the stack."

---

### Slide 7 — The Java Stack

> Why Java? Because there are millions of Java developers in enterprises. They already have Java teams, Java CI/CD, Java expertise. Asking them to learn Go just to build an operator is a hard sell.
>
> Here's the stack. At the bottom is the Kubernetes API. On top of that sits the Fabric8 Kubernetes Client — the foundation. It gives you a fluent API for CRUD operations, watching resources, and working with CRDs.
>
> On top of Fabric8 sits the Java Operator SDK — JOSDK. It adds the reconciliation loop, event handling, dependent resource management, and all the production infrastructure you need.
>
> And on top of that sits your operator — your business logic and domain knowledge.

**Transition:** "Let's start with the foundation."

---

### Slide 8 — Fabric8 Kubernetes Client

> Fabric8 is the industry standard. Let me highlight what makes it stand out.
>
> First — and this is a key differentiator — multiple HTTP client implementations. OkHttp, Vert.x, JDK HttpClient, Jetty. You pick what fits your stack. No other Java K8s client gives you this flexibility.
>
> Fluent DSL — every API call is chainable and readable. First-class OpenShift support — the official Kubernetes Java client doesn't have this. CRD support with code generation. Extensions for Knative, Tekton, Istio. And Watch & Informers for real-time event handling with caching.
>
> Now, I want to call special attention to the testing story. *(point to the highlighted card)* This is not available in the official Kubernetes Java client. Fabric8 gives you a Mock Server, CRUD mode, and a Kube API Test Server — fast, robust testing for your operators without spinning up a real cluster.
>
> In the AI-driven development era, where you're iterating faster than ever, rapid feedback loops from testing matter more than they ever have. You can test your operators locally, get instant feedback, and ship with confidence.

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

**Transition:** "Let's build an operator. Step one — define your Custom Resource Definition."

---

### Slide 12 — Custom Resource Definition

> This is your CRD as a Java class. Three annotations — Group, Version, ShortNames. Extend CustomResource with your Spec and Status types. Implement Namespaced. That's the CRD definition.
>
> Below that is the Spec class — plain Java. Image, replicas, database config, exposed flag, hostname. Just fields.
>
> And here's the beautiful part — the Fabric8 Kubernetes Client CRD Generator takes this Java class and generates the full YAML schema automatically. You don't write CRD YAML by hand.

**Transition:** "Now let's see what your users actually write."

---

### Slide 13 — Custom Resource — What Users Write

> On the left is the simplest form — a basic Custom Resource. Image, replicas, exposed. That's enough to deploy a microservice with a Deployment, Service, and Ingress.
>
> On the right is the same CR with more spec — add a database and the operator provisions PostgreSQL. Add autoscaling and you get an HPA. Set a hostname and you get Ingress routing.
>
> Same resource type, same operator — the spec just grows as your needs grow. And the operator creates all the underlying Kubernetes resources automatically.

**Transition:** "Here's what a full production-ready CR looks like."

---

### Slide 14 — Full Custom Resource — Production Example

> This is a real CR from our microservice-operator. Notice the `database` section — when you set this, the operator automatically provisions a PostgreSQL StatefulSet with a PVC, generates random credentials into a Secret, creates a headless Service, and injects the credentials into the app Deployment.
>
> No `kubectl create secret`. Ever. The operator handles the full lifecycle.
>
> You also see autoscaling and ingress configuration — all declarative, all managed by the operator.

**Transition:** "Now let's see the reconciler that makes this work."

---

### Slide 15 — Implementing the Reconciler

> This is the real reconciler from the microservice-operator. Look at the `@Workflow` annotation — it declares all the dependent resources and their dependency order. ConfigMap first, then Deployment depends on ConfigMap, Service depends on Deployment, and so on.
>
> The `reconcile()` method itself is tiny — it just reads the Deployment's ready replicas from the cache and patches the status. All the heavy lifting — creating Deployments, Services, ConfigMaps — is handled by the dependent resources.
>
> This is the beauty of JOSDK. Your reconciler stays clean and focused.

**Transition:** "Let me show you how dependent resources work."

---

### Slide 16 — Dependent Resources

> Each dependent resource is a class that manages one Kubernetes resource. You extend `CRUDKubernetesDependentResource` and implement one method — `desired()`.
>
> You return what the Deployment SHOULD look like. The framework compares it with what actually exists in the cluster and takes action — create if missing, update if different, leave alone if matching.
>
> Owner references are set automatically, so when you delete the custom resource, all dependent resources are garbage-collected. You don't write cleanup code.

**Transition:** "Now let's talk about doing this right in production."

---

### Slide 17 — Patterns and Best Practices

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

### Slide 18 — Error Handling

> JOSDK gives you `updateErrorStatus` — when reconciliation fails, this method is called so you can write the error to the status subresource. Your users can see what went wrong by looking at the custom resource's status.
>
> For cleanup, you implement `cleanup()`. But notice it's one line — `defaultDelete()`. Because we set owner references on all dependent resources, Kubernetes garbage-collects them automatically when the custom resource is deleted.
>
> This is production-grade error handling with minimal code.

**Transition:** "You might be wondering — is anyone actually using this in production?"

---

### Slide 19 — Who's Using It in Production?

> Absolutely. The Keycloak Operator — the official one — is built with Quarkus and JOSDK. That's a major CNCF-adjacent identity platform.
>
> Apache Flink's Kubernetes Operator — described as the "market leader" among Flink operators — uses JOSDK. Apache Spark is building their operator with it too.
>
> Strimzi uses it for the Kafka ecosystem. Debezium for Change Data Capture. Glasskube for Kubernetes package management.
>
> This is not a toy framework. It's battle-tested at scale.

**Transition:** "Here are all the links you need."

---

### Slide 20 — Resources

> Everything is open source. The Fabric8 Client and JOSDK are both on GitHub. The JOSDK website has excellent documentation.
>
> The demo project — microservice-operator — is the code we've been looking at today. Clone it, run it with Kind or Minikube, and try it yourself.
>
> And my blog has more posts about operators and Kubernetes with Java.

**Transition:** "Now let me show you all of this working live."

---

### Slide 21 — DEMO

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

### Slide 22 — Thanks

> Thank you! The code is all on GitHub. You can find me on Twitter at @ashish___thakur or connect on LinkedIn. I'm happy to chat about Fabric8, JOSDK, or anything Kubernetes and Java.
>
> Questions?

---

## Timing Guide

| Section | Slides | Target Time |
|---------|--------|-------------|
| Intro & Problem | 1-6 | 8 min |
| Java Tooling | 7-11 | 8 min |
| Build It | 12-18 | 10 min |
| Proof & Close | 19-20 | 3 min |
| Demo | 21 | 5-7 min |
| Q&A | — | 5-10 min |
| **Total** | | **~35-45 min** |

## Tips

- **Before the talk:** Have the Kind cluster pre-created and the operator pre-built. Demo failures are the #1 confidence killer.
- **Slide 6 is the hook:** The Real-World Example is where the audience goes "oh, that's useful." Emphasize that the resources still exist — the operator manages them, they don't disappear.
- **Slide 8 — Testing:** Don't rush past the testing card. This is a unique differentiator. Pause and let the AI/feedback-loop point land.
- **Slides 12-16 are dense:** Don't read the code line by line. Highlight the key annotations and the `desired()` method pattern. The audience can read the details later.
- **Demo fallback:** If the live demo fails, have a pre-recorded terminal session (asciinema) ready to play.
- **Q&A bait:** Mention "we chose JOSDK over Go's Operator SDK" early — someone will ask why during Q&A, and that's a great discussion.
