---
title: 'Your Tests Are Your Spec: Why TDD and Fast CI/CD Are the Real Enablers of Agentic Software Development'
date: 2026-09-09
description: 'Why TDD and fast CI/CD pipelines are the essential infrastructure for safe and productive AI-assisted software development'
event: 'TechGENIE 2026'
tags: [ 'tdd', 'ci-cd', 'agentic-sdlc', 'testing', 'fabric8', 'java' ]
theme: black
fonts:
  - 'JetBrains Mono:wght@400;600;700'
  - 'Inter:wght@400;500'
footer:
  left: 'ashish thakur · red hat'
  right: 'techgenie 2026 · september 2026'
customCSS: |
  .reveal .slides { text-align: left; }
  .reveal .controls { color: #22d3ee; }
  .reveal .progress { color: #22d3ee; height: 2px; }
revealOptions: |
  transition: 'none',
  controlsTutorial: false,
  center: false,
  width: 1280,
  height: 720,
  margin: 0.02,
  minScale: 0.2,
  maxScale: 2.0,
---

<section class="title-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">talk.md</span></span>
<span class="term-act">01</span>
</div>

<div class="title-body">

# Your Tests Are Your Spec

<div class="subtitle">// Why TDD and Fast CI/CD Are the Real Enablers of Agentic Software Development</div>

<div class="author-block">
<span class="name">Ashish Thakur</span><br>
<span class="role">Senior Software Engineer · Red Hat</span>
</div>

<div class="event-tag">TechGENIE 2026 · 09 September 2026</div>

</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">whoami.md</span></span>
<span class="term-act">02</span>
</div>

## About Me

<div class="gh-profile">

<div class="gh-sidebar">
<img class="gh-avatar-circle" src="/avatar.jpeg" alt="Ashish Thakur">
<div class="gh-profile-name">Ashish Thakur</div>
<div class="gh-profile-login">ash-thakur-rh</div>
<div class="gh-profile-bio">Senior Software Engineer at Red Hat. Open source maintainer &amp; conference speaker.</div>
<hr class="gh-profile-divider">
<div class="gh-profile-meta">
<div class="gh-profile-meta-row"><span>🏢</span><span>Red Hat</span></div>
<div class="gh-profile-meta-row"><span>📍</span><span>India</span></div>
<div class="gh-profile-meta-row"><span>🐦</span><a class="gh-profile-meta-link" href="https://twitter.com/ashish___thakur" target="_blank">@ashish___thakur</a></div>
<div class="gh-profile-meta-row"><span>🌐</span><a class="gh-profile-meta-link" href="https://www.ashthakur.in/blog" target="_blank">ashthakur.in/blog</a></div>
<div class="gh-profile-meta-row"><span>🔗</span><a class="gh-profile-meta-link" href="https://github.com/ash-thakur-rh" target="_blank">github.com/ash-thakur-rh</a></div>
</div>
</div>

<div class="gh-main-col">
<div class="gh-pinned-label">Pinned</div>
<div class="gh-repo-grid">
<div class="gh-repo-card">
<div class="gh-repo-card-name"><a href="https://github.com/fabric8io/kubernetes-client" target="_blank">fabric8io / kubernetes-client</a></div>
<div class="gh-repo-card-desc">Java client for Kubernetes &amp; OpenShift — 75+ Maven modules, multiple HTTP backends (OkHttp · Vert.x · JDK · Jetty)</div>
<div class="gh-repo-card-footer">
<span><span class="gh-lang-dot" style="background:#b07219"></span> Java</span>
<span>★ 3.7k</span>
<span>⑂ 1.5k</span>
<span class="c-bright">&gt;1M dl/mo</span>
</div>
</div>
<div class="gh-repo-card">
<div class="gh-repo-card-name"><a href="https://github.com/eclipse-jkube/jkube" target="_blank">eclipse-jkube / jkube</a></div>
<div class="gh-repo-card-desc">Build &amp; deploy Java apps to Kubernetes/OpenShift — Maven &amp; Gradle plugins, zero-config Docker image build</div>
<div class="gh-repo-card-footer">
<span><span class="gh-lang-dot" style="background:#b07219"></span> Java</span>
<span>★ 851</span>
<span>⑂ 552</span>
</div>
</div>
</div>
<div class="gh-highlights-hdr">Highlights</div>
<div class="gh-highlights">
<div class="gh-highlight-row"><span class="hl-val c-green">★ 3.7k</span><span>stars · fabric8 kubernetes client</span></div>
<div class="gh-highlight-row"><span class="hl-val c-cyan">&gt;1M / mo</span><span>Maven downloads across all modules</span></div>
<div class="gh-highlight-row"><span class="hl-val c-bright">11 yrs</span><span>open source maintenance &amp; contribution</span></div>
<div class="gh-highlight-row"><span class="hl-val c-yellow">31</span><span>flaky tests fixed → 5 real production bugs found</span></div>
<div class="gh-highlight-row"><span class="hl-val c-dim">powers</span><span>Quarkus · Spring Cloud Kubernetes · Apache Flink · JOSDK</span></div>
</div>
</div>

</div>

</section>

<!-- ══════════════════════════════════════════ ACT 01 ══ -->

<section class="act-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Act01.java</span></span>
<span class="term-act">03</span>
</div>

<div class="act-body">

<div class="reactor-box">
<div class="reactor-hdr">[INFO] Building io.techgenie:talk 1.0.0-SNAPSHOT</div>
<div class="reactor-act active"><span>▶</span><span class="ra-name">act-01 · the-shift ................................</span><span>BUILDING ...</span></div>
<div class="reactor-act pending"><span>·</span><span class="ra-name">act-02 · tests-are-your-spec ......................</span><span>PENDING</span></div>
<div class="reactor-act pending"><span>·</span><span class="ra-name">act-03 · fast-feedback-loops ......................</span><span>PENDING</span></div>
<div class="reactor-act pending"><span>·</span><span class="ra-name">act-04 · patterns .....................................</span><span>PENDING</span></div>
</div>

<div class="act-eyebrow">// act 01</div>
<h2 class="act-heading">The *Shift*</h2>

</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">TheShift.java</span></span>
<span class="term-act">act 01 · 04</span>
</div>

## AI writes code. You verify it.

<div class="diff-compare" style="margin-top:.45em">
<div class="diff-panel">
<div class="diff-panel-header bad">✗ &nbsp;Old World</div>
<div class="diff-panel-body">
<p>You write the code.<br>You write the tests.<br>Tests validate <em>your</em> work.</p>
<p style="margin-top:.6em;color:#333">bottleneck: developer writing speed</p>
</div>
</div>
<div class="diff-panel">
<div class="diff-panel-header good">▶ &nbsp;Agentic World</div>
<div class="diff-panel-body">
<p>AI writes the code — fast, confident,<br>syntactically correct, <span class="c-red">sometimes subtly wrong</span>.</p>
<p style="margin-top:.6em;color:#555">bottleneck: <span class="c-cyan">verification infrastructure</span></p>
</div>
</div>
</div>

<div class="t-callout fragment" style="margin-top:.45em">
<p><span class="c-bright">The developer's role shifts</span> — from writing every line to <span class="c-cyan">defining intent</span> and <span class="c-cyan">verifying correctness</span>. The agent implements; you set the bar it must clear.</p>
</div>

<div class="takeaway fragment">without verification infrastructure, AI-assisted development is <em>AI-assisted gambling</em></div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">TheAmplifier.java</span></span>
<span class="term-act">act 01 · 05</span>
</div>

## AI amplifies what's already there

<div class="diff-compare">
<div class="diff-panel">
<div class="diff-panel-header bad">✗ &nbsp;Poor hygiene → slop, faster</div>
<div class="diff-panel-body">
<ul>
<li>Huge PRs with no baby steps</li>
<li>No behavioral tests or spec docs</li>
<li>Vague issues, no acceptance criteria</li>
<li>Slow, flaky CI pipeline</li>
<li>Problems hidden for months → exposed in hours</li>
</ul>
</div>
</div>
<div class="diff-panel">
<div class="diff-panel-header good">✓ &nbsp;Good hygiene → acceleration</div>
<div class="diff-panel-body">
<ul>
<li>Small, atomic, reviewable PRs</li>
<li>Behavioral tests that enforce intent</li>
<li>Well-defined issues with acceptance criteria</li>
<li>Fast, stable CI with real feedback</li>
<li>Guardrails catch AI mistakes before prod</li>
</ul>
</div>
</div>
</div>

<div class="takeaway fragment">"Legacy is not age. Legacy is <em>unsafe change</em>." — an AI-assisted codebase without tests becomes legacy fast</div>

</section>

<!-- ══════════════════════════════════════════ ACT 02 ══ -->

<section class="act-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Act02.java</span></span>
<span class="term-act">06</span>
</div>

<div class="act-body">

<div class="reactor-box">
<div class="reactor-hdr">[INFO] Building io.techgenie:talk 1.0.0-SNAPSHOT</div>
<div class="reactor-act done"><span>✓</span><span class="ra-name">act-01 · the-shift ................................</span><span>SUCCESS [03:30]</span></div>
<div class="reactor-act active"><span>▶</span><span class="ra-name">act-02 · tests-are-your-spec ......................</span><span>BUILDING ...</span></div>
<div class="reactor-act pending"><span>·</span><span class="ra-name">act-03 · fast-feedback-loops ......................</span><span>PENDING</span></div>
<div class="reactor-act pending"><span>·</span><span class="ra-name">act-04 · patterns .....................................</span><span>PENDING</span></div>
</div>

<div class="act-eyebrow">// act 02</div>
<h2 class="act-heading">Tests Are *Your Spec*</h2>

</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">TDDThenVsNow.java</span></span>
<span class="term-act">act 02 · 07</span>
</div>

## TDD: the practice that changed its meaning

<div class="diff-compare">
<div class="diff-panel" style="flex:1">
<div class="diff-panel-header" style="color:#58a6ff;background:rgba(88,166,255,.05)">Traditional TDD</div>
<div class="diff-panel-body">
<div class="tdd-steps">
<div class="tdd-step red"><span class="step-num">①</span><span class="step-text">Write a failing test</span></div>
<div class="tdd-step green"><span class="step-num">②</span><span class="step-text">Write code to make it pass</span></div>
<div class="tdd-step blue"><span class="step-num">③</span><span class="step-text">Refactor</span></div>
</div>
<p style="margin-top:.5em" class="c-dim">Developer drives all three steps.</p>
<p class="c-dim">TDD is a <em>development loop</em>.</p>
</div>
</div>
<div class="diff-panel" style="flex:1">
<div class="diff-panel-header" style="color:#22d3ee;background:rgba(34,211,238,.05)">Agentic TDD</div>
<div class="diff-panel-body">
<div class="tdd-steps">
<div class="tdd-step cyan"><span class="step-num">①</span><span class="step-text">Write a failing test<span class="step-aside">(you define intent)</span></span></div>
<div class="tdd-step blue"><span class="step-num">②</span><span class="step-text">AI writes code to pass it</span></div>
<div class="tdd-step green"><span class="step-num">③</span><span class="step-text">CI validates it</span></div>
</div>
<p style="margin-top:.5em" class="c-dim">TDD shifts from a development discipline to a</p>
<p><span class="c-cyan">design communication tool</span> — authoring intent for both humans and machines.</p>
</div>
</div>
</div>

</section>

<section class="content-slide stmt-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Spec.java</span></span>
<span class="term-act">act 02 · 08</span>
</div>

<div class="stmt-body">
<div class="stmt-group">
<div class="stmt-comment">// specs</div>
<div class="stmt-text">explain <em>intent</em></div>
</div>
<div class="stmt-group">
<div class="stmt-comment">// tests</div>
<div class="stmt-text stmt-text-2">enforce <em>it</em></div>
</div>
</div>

<div class="stmt-sub fragment">
<span class="c-dim">A prose spec is advisory.</span>&nbsp;
<span class="c-bright">A test is enforced on every commit.</span><br>
<span class="c-dim">When the spec drifts, no one notices.</span>&nbsp;
<span class="c-cyan">When the test fails, the build fails.</span>
</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">TestAsSpec.java</span></span>
<span class="term-act">act 02 · 09</span>
</div>

## A failing test is a machine-readable spec

```java

@Nested
@DisplayName("when creating a pod")
class WhenCreatingAPod {

  @Test
  @DisplayName("assigns the pod to the requested namespace")
  void assignsPodToNamespace() { ...}

  @Test
  @DisplayName("rejects creation if namespace does not exist")
  void rejectsCreationForMissingNamespace() { ...}
}
```

<div class="fail-output">
<span class="fail">✗ BUILD FAILED</span> <span class="muted">— PodServiceTest</span><br>
&nbsp;&nbsp;<span class="muted">when creating a pod</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;<span class="fail">✗ rejects creation if namespace does not exist</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span class="dimmer">expected: status 404 · actual: status 201</span>
</div>

<div class="takeaway">the failing test reads like a sentence — top to bottom. <em>That sentence is your spec.</em> Text docs are advisory; tests are enforced on every commit.</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">BlackBoxVsWhiteBox.java</span></span>
<span class="term-act">act 02 · 10</span>
</div>

## Write tests that survive AI refactors

<div class="diff-compare">
<div class="diff-panel">
<div class="diff-panel-header bad">✗ &nbsp;White-box — mocks internals</div>
<div class="diff-panel-body">
<pre><code class="language-java">@Mock KubernetesClient client;
@Test void deletesPod() {
    service.delete("foo");
    // agent renames pods()→resources()
    // agent rewrites this mock in lockstep
    // nothing pins the actual contract
    verify(client.pods()
           .withName("foo")).delete();
}</code></pre>
<p class="note">Test and impl travel together. No guardrail survives the refactor.</p>
</div>
</div>
<div class="diff-panel">
<div class="diff-panel-header good">✓ &nbsp;Black-box — asserts outcomes</div>
<div class="diff-panel-body">
<pre><code class="language-java">@QuarkusTest @WithKubernetesTestServer
class PodBehaviorIT {
  @Test
  @DisplayName("DELETE removes the pod")
  void removesPodFromCluster() {
    k8s.pods().resource(pod).create();
    given().delete("/api/v1/pods/foo")
           .then().statusCode(204);
    assertThat(k8s.pods()
        .withName("foo").get()).isNull();
  }
}</code></pre>
<p class="note c-green">Zero test edits after agent refactor. The outcome is the contract.</p>
</div>
</div>
</div>

</section>

<!-- ══════════════════════════════════════════ ACT 03 ══ -->

<section class="act-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Act03.java</span></span>
<span class="term-act">10</span>
</div>

<div class="act-body">

<div class="reactor-box">
<div class="reactor-hdr">[INFO] Building io.techgenie:talk 1.0.0-SNAPSHOT</div>
<div class="reactor-act done"><span>✓</span><span class="ra-name">act-01 · the-shift ................................</span><span>SUCCESS [03:30]</span></div>
<div class="reactor-act done"><span>✓</span><span class="ra-name">act-02 · tests-are-your-spec ......................</span><span>SUCCESS [04:20]</span></div>
<div class="reactor-act active"><span>▶</span><span class="ra-name">act-03 · fast-feedback-loops ......................</span><span>BUILDING ...</span></div>
<div class="reactor-act pending"><span>·</span><span class="ra-name">act-04 · patterns .....................................</span><span>PENDING</span></div>
</div>

<div class="act-eyebrow">// act 03</div>
<h2 class="act-heading">Fast *Feedback Loops*</h2>

</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">SpeedMismatch.java</span></span>
<span class="term-act">act 03 · 11</span>
</div>

## AI moves at machine speed. CI doesn't.

<div class="gh-window" style="margin-top:.5em">
<div class="gh-topbar">
<span class="gh-repo">fabric8io/kubernetes-client</span><span class="gh-sep">/</span><span>Pull requests</span>
<span class="gh-sep">·</span><span>Open (4) — AI-generated in the last 10 minutes</span>
</div>
<div class="gh-pr-list">
<div class="gh-pr-row">
<span class="gh-pr-badge open">Open</span>
<span class="gh-pr-name">#842 · feat: add retry logic for transient API failures</span>
<span class="gh-pr-ci ci-wait">⏳ CI queued · 34m ago</span>
</div>
<div class="gh-pr-row">
<span class="gh-pr-badge open">Open</span>
<span class="gh-pr-name">#843 · fix: handle null namespace in informer shutdown path</span>
<span class="gh-pr-ci ci-run">● CI running · 28m elapsed</span>
</div>
<div class="gh-pr-row">
<span class="gh-pr-badge open">Open</span>
<span class="gh-pr-name">#844 · test: add missing coverage for SSL body-stream edge case</span>
<span class="gh-pr-ci ci-run">● CI running · 12m elapsed</span>
</div>
<div class="gh-pr-row">
<span class="gh-pr-badge open">Open</span>
<span class="gh-pr-name">#845 · refactor: extract shared serial executor helper</span>
<span class="gh-pr-ci ci-wait">⏳ CI queued · just now</span>
</div>
</div>
</div>

<div class="t-callout danger fragment" style="margin-top:.45em">
<p><span class="c-red">A 35-minute CI pipeline turns an AI agent into an expensive queue.</span><br>
The agent spawns dozens of PRs per hour — they all wait in the same line. Your CI is the rate-limiter on your entire agentic workflow.</p>
</div>

<div class="takeaway fragment"><em>fast CI/CD is a multiplier, not a luxury</em> — sub-10-minute pipelines unlock the real productivity gains of agentic development</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Fabric8Project.java</span></span>
<span class="term-act">act 03 · 12</span>
</div>

## Case study: fabric8 Kubernetes Client

<div class="t-row">
<div class="t-box accent-cyan" style="flex:1.4">
<h3>Project</h3>
<p><span class="c-cyan">fabric8io/kubernetes-client</span></p>
<p><span class="c-green">★ 3.7k</span> &nbsp;·&nbsp; <span class="c-bright">&gt;1M downloads/month</span> &nbsp;·&nbsp; 11 years</p>
<p style="margin-top:.4em">75+ Maven modules · multiple HTTP backends<br>(OkHttp · Vert.x · JDK HttpClient · Jetty)</p>
<p style="margin-top:.4em">Powers: <span class="c-bright">Quarkus</span> · <span class="c-bright">Spring Cloud K8s</span> · <span class="c-bright">Apache Flink</span> · <span class="c-cyan">JOSDK</span></p>
</div>
<div class="t-box accent-red" style="flex:1">
<h3 class="red">The CI problem</h3>
<p><span class="c-red">30–40 minutes</span> per PR.</p>
<p>Sequential jobs. Flaky tests retried 2–5× per run.</p>
<p style="margin-top:.4em">Every contributor.<br>Every PR.<br>Every day.</p>
<p style="margin-top:.4em">The CI became a throughput cap on all velocity — including AI-assisted work.</p>
</div>
</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">CIPipeline.java</span></span>
<span class="term-act">act 03 · 13</span>
</div>

## Before and after: the pipeline transformation

<div class="diff-compare">
<div class="diff-panel" style="flex:1.1">
<div class="diff-panel-header bad">✗ &nbsp;Before — sequential, slow, flaky</div>
<div class="diff-panel-body" style="padding:0">
<div class="gh-window" style="border:none;border-radius:0">
<div class="gh-topbar">
<span class="gh-repo">fabric8io/kubernetes-client</span><span class="gh-sep">/</span><span>Actions</span><span class="gh-sep">·</span><span>Run #1823 · PR #7800</span>
</div>
<div class="gh-wf-hdr">
<span class="gh-dot fail">✗</span>
<span class="gh-wf-hdr-title">CI — 38m 42s · 2 failed jobs</span>
<span class="gh-wf-hdr-note">sequential · no cache</span>
</div>
<div class="gh-jobs">
<div class="gh-job"><span class="gh-dot fail">✗</span><span class="gh-job-name">unit-tests</span><span class="gh-job-dur">18m 23s</span><span class="gh-job-note">× 2 retries</span></div>
<div class="gh-job"><span class="gh-dot skip">○</span><span class="gh-job-name">unit-tests (retry 1)</span><span class="gh-job-dur">18m 41s</span><span class="gh-job-note">still flaky</span></div>
<div class="gh-job"><span class="gh-dot pass">✓</span><span class="gh-job-name">build</span><span class="gh-job-dur">8m 11s</span></div>
<div class="gh-job"><span class="gh-dot pass">✓</span><span class="gh-job-name">style-check</span><span class="gh-job-dur">5m 04s</span></div>
<div class="gh-job"><span class="gh-dot pass">✓</span><span class="gh-job-name">sonar</span><span class="gh-job-dur">7m 04s</span></div>
</div>
</div>
</div>
</div>
<div class="diff-panel" style="flex:1">
<div class="diff-panel-header good">✓ &nbsp;What we changed</div>
<div class="diff-panel-body">
<ul>
<li><span class="c-cyan">KubernetesMockServer</span> — no real cluster in unit tests<br><span class="c-red mono" style="font-size:.85em">~50s/class</span> → <span class="c-green mono" style="font-size:.85em">~1s/class</span></li>
<li style="margin-top:.4em"><span class="c-cyan">Parallel matrix</span> — Java versions &amp; HTTP backends run concurrently</li>
<li style="margin-top:.4em"><span class="c-cyan">Maven dep caching</span> — eliminated cold-start download cost</li>
<li style="margin-top:.4em"><span class="c-cyan">Selective E2E</span> — KinD cluster tests on PR label or main only</li>
</ul>
</div>
</div>
</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Results.java</span></span>
<span class="term-act">act 03 · 14</span>
</div>

## Real numbers — ~40 PRs across 4 epics

<table class="ci-table">
<tr>
<th>module / job</th>
<th>before</th>
<th>after</th>
<th>cut</th>
</tr>
<tr>
<td class="module">kubernetes-tests (surefire)</td>
<td class="before">266s</td>
<td class="after">109s</td>
<td class="cut">−59%</td>
</tr>
<tr>
<td class="module">kubernetes-client-api (surefire)</td>
<td class="before">84s</td>
<td class="after">31s</td>
<td class="cut">−62%</td>
</tr>
<tr>
<td class="module">kubernetes-client (surefire)</td>
<td class="before">153s</td>
<td class="after">43s</td>
<td class="cut">−72%</td>
</tr>
<tr>
<td class="module">Windows build (wall-clock)</td>
<td class="before">~40 min (flaky, retried 2–5×)</td>
<td class="after">~22 min (stable)</td>
<td class="cut">−45%</td>
</tr>
</table>

<div class="metrics-row">
<div class="metric-tile">
<span class="metric-num cyan">−45%</span>
<span class="metric-label">billable CI minutes<br>per PR</span>
</div>
<div class="metric-tile">
<span class="metric-num green">~10 min</span>
<span class="metric-label">critical-path CI<br>(happy path)</span>
</div>
<div class="metric-tile">
<span class="metric-num blue">$0.005/min</span>
<span class="metric-label">runner cost after<br>macOS → ARM64</span>
</div>
</div>

<div class="gh-merge-box">
<div class="gh-topbar"><span class="gh-repo">fabric8io/kubernetes-client</span><span class="gh-sep">/</span><span>Pull requests</span></div>
<div class="gh-pr-title-row">
<span class="gh-badge merged">Merged</span>
<span class="gh-pr-title-text">#7812 · test: use ephemeral ports in httpclient-vertx SSL/body-stream tests</span>
</div>
<div class="gh-checks-hdr pass">✓ &nbsp;All checks have passed</div>
<div class="gh-check-row"><span class="gh-check-icon pass">✓</span><span class="gh-check-name">CI / unit-tests (Java 21 · OkHttp)</span><span class="gh-check-dur">1m 49s</span></div>
<div class="gh-check-row"><span class="gh-check-icon pass">✓</span><span class="gh-check-name">CI / unit-tests (Java 17 · Vert.x)</span><span class="gh-check-dur">1m 31s</span></div>
<div class="gh-check-row"><span class="gh-check-icon pass">✓</span><span class="gh-check-name">CI / unit-tests (Java 21 · JDK HttpClient)</span><span class="gh-check-dur">1m 22s</span></div>
<div class="gh-check-row"><span class="gh-check-icon pass">✓</span><span class="gh-check-name">CI / build · sonar</span><span class="gh-check-dur">48s</span></div>
<div class="gh-merge-footer">
<span class="gh-merge-footer-msg">Merged by ash-thakur-rh into main · ±4 files · 2 modules · 0 API changes</span>
<span class="gh-merge-btn">✓ Merged</span>
</div>
</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">HiddenBugs.java</span></span>
<span class="term-act">act 03 · 15</span>
</div>

## Fixing flaky tests surfaced real production bugs

<div class="gh-window" style="margin:.15em 0 .35em">
<div class="gh-topbar"><span class="gh-repo">fabric8io/kubernetes-client</span><span class="gh-sep">/</span><span>Actions</span><span class="gh-sep">·</span><span>31 flaky issues closed in ~8 weeks · <span style="color:#f85149">5+ were real production bugs</span></span></div>
<div class="gh-wf-hdr">
<span class="gh-dot fail">✗</span>
<span class="gh-wf-hdr-title">CI — tests started failing deterministically after flake fix</span>
<span class="gh-wf-hdr-note">not flaky · not noise · real bugs</span>
</div>
<div class="gh-jobs">
<div class="gh-job"><span class="gh-dot fail">✗</span><span class="gh-job-name">ExecWebSocketListenerTest · testHandleExitStatus</span><span class="gh-job-dur">exit code dropped on peer-close race → #7700</span></div>
<div class="gh-job"><span class="gh-dot fail">✗</span><span class="gh-job-name">ExecListenerTest · testOnCloseRace</span><span class="gh-job-dur">callers block forever on transport drop → #7779</span></div>
<div class="gh-job"><span class="gh-dot fail">✗</span><span class="gh-job-name">SharedInformerTest · testStopRace</span><span class="gh-job-dur">NPE in SerialExecutor post-stop → #7716</span></div>
</div>
</div>

<div class="t-row">
<div class="t-box accent-red">
<h3 class="red">#7700 — Exit code silently dropped</h3>
<p><span class="c-blue">ExecWebSocketListener.onError</span> dropped the process exit code on peer-close race. <span class="c-dim">cleanUpOnce()</span> shut down <span class="c-dim">SerialExecutor</span>, discarding the queued <span class="c-dim">handleExitStatus</span>. Callers saw silent failures.</p>
</div>
<div class="t-box accent-red">
<h3 class="red">#7779 — Callers blocked forever</h3>
<p><span class="c-blue">ExecListener.onClose/onFailure</span> lost on close-handshake race. Callers blocked on a latch indefinitely on transport drop. No timeout, no error signal.</p>
</div>
<div class="t-box accent-red">
<h3 class="red">#7716 — NPE after stop()</h3>
<p><span class="c-blue">SharedProcessor.distribute()</span> threw NPE when called after <span class="c-dim">informer.stop()</span> — <span class="c-dim">SerialExecutor.execute()</span> invoked post-shutdown.</p>
</div>
</div>

<div class="takeaway">when AI makes everything move fast, tech debt shows up as a <em>throughput cap</em> — and flaky tests are the canary</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">FeedbackPyramid.java</span></span>
<span class="term-act">act 03 · 16</span>
</div>

## The feedback pyramid: many fast, few slow

<div class="pyramid">
<div class="pyr-row t1">
<div class="pyr-speed">~1s / class</div>
<div class="pyr-tool">In-process mock · <code>@EnableKubernetesMockClient(https=false)</code> · Fabric8 KubernetesMockServer</div>
<div class="pyr-when">every save</div>
</div>
<div class="pyr-row t2">
<div class="pyr-speed">~3s / class</div>
<div class="pyr-tool">Real apiserver · <code>@EnableKubeAPIServer</code> · Fabric8 kube-api-test (envtest)</div>
<div class="pyr-when">every save</div>
</div>
<div class="pyr-row t3">
<div class="pyr-speed">~5s / class</div>
<div class="pyr-tool">App booted in-process · <code>@QuarkusTest @WithKubernetesTestServer</code></div>
<div class="pyr-when">every commit</div>
</div>
<div class="pyr-row t4">
<div class="pyr-speed">~50s / class</div>
<div class="pyr-tool">KinD cluster · <code>Testcontainers + KinD</code> · real container runtime, real kubelet</div>
<div class="pyr-when">every PR</div>
</div>
<div class="pyr-row t5">
<div class="pyr-speed">~10 min / suite</div>
<div class="pyr-tool">Real dev cluster · <code>KubernetesClientBuilder</code> smoke tests · pre-release only</div>
<div class="pyr-when">pre-release</div>
</div>
</div>

<div class="takeaway" style="margin-top:.3em">many tests at the top, few at the bottom — <em>agent iterates where it's cheap</em></div>

</section>

<!-- ══════════════════════════════════════════ ACT 04 ══ -->

<section class="act-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Act04.java</span></span>
<span class="term-act">17</span>
</div>

<div class="act-body">

<div class="reactor-box">
<div class="reactor-hdr">[INFO] Building io.techgenie:talk 1.0.0-SNAPSHOT</div>
<div class="reactor-act done"><span>✓</span><span class="ra-name">act-01 · the-shift ................................</span><span>SUCCESS [03:30]</span></div>
<div class="reactor-act done"><span>✓</span><span class="ra-name">act-02 · tests-are-your-spec ......................</span><span>SUCCESS [04:20]</span></div>
<div class="reactor-act done"><span>✓</span><span class="ra-name">act-03 · fast-feedback-loops ......................</span><span>SUCCESS [06:10]</span></div>
<div class="reactor-act active"><span>▶</span><span class="ra-name">act-04 · patterns .....................................</span><span>BUILDING ...</span></div>
</div>

<div class="act-eyebrow">// act 04</div>
<h2 class="act-heading">*Patterns* to Start Today</h2>

</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Pattern1.java</span></span>
<span class="term-act">act 04 · 18</span>
</div>

## Pattern 1: write tests that communicate intent

<div class="diff-compare">
<div class="diff-panel" style="flex:1.1">
<div class="diff-panel-header" style="color:#58a6ff;background:rgba(88,166,255,.05)">Use @DisplayName as your spec language</div>
<div class="diff-panel-body">
<pre><code class="language-java">@Nested
@DisplayName("PodService")
class PodServiceTest {

@Nested @DisplayName ("when deleting a pod")
class WhenDeletingAPod {

    @Test
    @DisplayName("removes it from the cluster")
    void removesPodFromCluster() { ... }

    @Test
    @DisplayName("returns 404 for unknown pod")
    void returnsNotFoundForUnknownPod() { ... }

} }</code></pre>
</div>
</div>
<div class="diff-panel">
<div class="diff-panel-header" style="color:#22d3ee;background:rgba(34,211,238,.05)">The AI reads this as a contract</div>
<div class="diff-panel-body">
<p>An AI agent given this test hierarchy understands exactly what to implement — not from a prose comment, but from <span class="c-cyan">code that runs on every commit</span>.</p>
<p style="margin-top:.5em"><span class="c-bright">Text docs are advisory.</span><br>Tests push back.</p>
<p style="margin-top:.5em;color:#333">// a failing @DisplayName reads like a sentence top to bottom</p>
<p style="margin-top:.5em">Fix one test this week — rename it to describe what it guards, not how it works.</p>
</div>
</div>
</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Pattern2.java</span></span>
<span class="term-act">act 04 · 19</span>
</div>

## Pattern 2: build your fast feedback loop first

<div class="t-row">
<div class="t-box accent-cyan">
<h3>Invest in in-process mocks</h3>
<p>For Kubernetes-facing code, <span class="c-blue">KubernetesMockServer</span> gives you <span class="c-cyan">~1s/class</span> — no cluster, no Docker, no waiting.</p>
<p style="margin-top:.4em">An agent running a 1-second test loop can make <span class="c-cyan">36 attempts</span> in the time it waits for one CI run.</p>
</div>
<div class="t-box">
<h3>Gate slow tests selectively</h3>
<p>E2E / KinD on <span class="c-blue">PR label</span> or main branch only.</p>
<p>Integration tests in a parallel matrix job.</p>
<p>Smoke tests pre-release, not every commit.</p>
<p style="margin-top:.4em">Goal: <span class="c-bright">sub-10-min critical path</span> on every PR.</p>
</div>
<div class="t-box">
<h3>Treat flaky tests as bugs</h3>
<p>Each flaky test is a retry cost multiplied across every contributor's every PR.</p>
<p style="margin-top:.4em">Under AI-driven velocity, flake debt compounds fast.</p>
<p style="margin-top:.4em"><span class="c-green">Turn one flaky test into a real, deterministic failing test this week.</span></p>
</div>
</div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">CompoundingAdvantage.java</span></span>
<span class="term-act">act 04 · 20</span>
</div>

## The compounding advantage

<div class="flywheel">
<div class="fw-step">
<div class="fw-num">1</div>
<h4>Write behavioral tests</h4>
<p>Tests define intent for both humans and AI agents. The spec is always current, always enforced.</p>
</div>
<div class="fw-step">
<div class="fw-num">2</div>
<h4>AI iterates faster</h4>
<p>Fast feedback loop = more agent cycles per hour. Mistakes caught in seconds, not 35 minutes later.</p>
</div>
<div class="fw-step">
<div class="fw-num">4</div>
<h4>Developer gains capacity</h4>
<p>Review scope, architecture, correctness — not syntax. You keep the judgment. AI takes the grind.</p>
</div>
<div class="fw-step">
<div class="fw-num">3</div>
<h4>Coverage compounds</h4>
<p>More iterations → more edge cases caught → higher confidence in AI output → agents trusted with more.</p>
</div>
</div>

<div class="takeaway">every improvement lowers the cost of the next change. <em>Teams that build this infrastructure first compound their advantage.</em></div>

</section>

<section class="content-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Takeaways.java</span></span>
<span class="term-act">act 04 · 21</span>
</div>

## Key takeaways

<div class="takeaway-list">
<div class="takeaway-item">
<div class="takeaway-num">01</div>
<div>
<strong>Tests are the new specification language.</strong>
<p>When AI agents write your code, your tests define <em>what</em> should be built. TDD shifts from a development discipline to a design communication tool — you're authoring intent for both humans and machines.</p>
</div>
</div>
<div class="takeaway-item">
<div class="takeaway-num">02</div>
<div>
<strong>Fast CI/CD is a multiplier, not a luxury.</strong>
<p>AI agents iterate at machine speed, but only if your feedback loop keeps up. A 35-minute pipeline turns an agent into an expensive queue. Sub-10-minute pipelines are what unlock the real gains — as we proved on fabric8.</p>
</div>
</div>
<div class="takeaway-item">
<div class="takeaway-num">03</div>
<div>
<strong>Trust requires verification infrastructure.</strong>
<p>AI-generated code is confident, syntactically correct, and sometimes subtly wrong. A comprehensive, fast test suite is the only scalable way to maintain confidence in code you didn't write line-by-line.</p>
</div>
</div>
</div>

</section>

<section class="content-slide resources">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Resources.md</span></span>
<span class="term-act">22</span>
</div>

## Resources

- **Fabric8 Kubernetes
  Client**: [github.com/fabric8io/kubernetes-client](https://github.com/fabric8io/kubernetes-client)
- **KubernetesMockServer (JUnit
  5)**: [github.com/fabric8io/kubernetes-client/blob/main/doc/junit5.md](https://github.com/fabric8io/kubernetes-client/blob/main/doc/junit5.md)
- **Java Operator SDK (JOSDK)**: [javaoperatorsdk.io](https://javaoperatorsdk.io)
- **Blog**: [ashthakur.in/blog](https://www.ashthakur.in/blog)
- **GitHub**: [github.com/ash-thakur-rh](https://github.com/ash-thakur-rh)

<div class="takeaway" style="margin-top:1em">AI takes the repetitive grind. <em>You keep the judgment.</em></div>

</section>

<section class="thanks-slide">

<div class="term-header">
<span><span class="term-path">~/dev/techgenie-2026</span><span class="term-sep"> · </span><span class="term-file">Thanks.java</span></span>
<span class="term-act">23</span>
</div>

<div class="thanks-body">

## Thanks!

<div class="contact">
<span class="name">Ashish Thakur</span><br>
<span class="role">Senior Software Engineer · Red Hat</span>
</div>

<div class="links" style="margin-top:.6em">
<span style="margin-right:1.4em">@ashish___thakur</span>
<span style="margin-right:1.4em">linkedin.com/in/ashish-thakur111</span>
<span><a href="https://www.ashthakur.in/blog">ashthakur.in/blog</a></span>
</div>

<div class="build-success">
[INFO] ─────────────────────────────────────────────<br>
[INFO] BUILD <span class="ok">SUCCESS</span><br>
[INFO] Total time: 15:00 min<br>
[INFO] ─────────────────────────────────────────────
</div>

</div>

</section>