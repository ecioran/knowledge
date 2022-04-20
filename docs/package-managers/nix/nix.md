---
title: Nix
---

# [Nix](https://github.com/NixOS/nix)

[nix.dev](https://nix.dev/) & [Nix One Pager](https://github.com/tazjin/nix-1p) are great places to start learning/using Nix. [How to learn Nix](https://ianthehenry.com/posts/how-to-learn-nix/) series is great too.

Looking into using [Nix Quick Install Action](https://github.com/nixbuild/nix-quick-install-action) in my projects. [devshell](https://github.com/numtide/devshell) is neat.

I used [Nix time](https://alexfedoseev.com/blog/post/nix-time) article to install Nix on my mac.

## Notes

- Nix never uses host dependencies, it always builds with exactly precise dependencies every time, and will always refer to them from then on.
- Nix lets you roll back changes atomically.
- nix-shell lets you make build environments that are totally reproducible across machines, and don’t interfere with each other. You can freely mix any number of libraries of versions or software on the same machine and they don’t conflict.
- With Ubuntu, every time you want to fix something with your car, you roll it into the garage, pop open the hood and get to work. It's intensive labour, results will vary, and undoing a change can be difficult.
  - With NixOS, it's like 3D printing a new car every time. You'll design a model, press a button, and the car gets built from scratch. If you don't like it, tweak the design a bit, and print a new car. If the new car breaks, just go back to the previous known-good one, which is already in your garage. You can even take the design documents to your friend and generate an exactly identical model.
- `sudo` command sets the wrong `$HOME`, have to use `sudo -i` for nix commands that need sudo.
- Nix is Turing complete language used for configuration and building packages.
- Can use nox, nix search, nix-repl, [nixOS packages](https://nixos.org/nixos/packages.html#) to search for packages.
- Think of Nix (the language) as an expression-based programming language where every program evaluates to a single (possibly complex) value; that resulting value is what is used as eg. the configuration of your system or a package, but it means that you can generate that value based on arbitrary logic and abstractions like you would with a regular programming language.
- As for domain-specific package managers, It Depends; it's possible with varying degrees of hackiness (and I definitely use eg. npm for development), but for a 'real' deployment - whether as a service on a server or as a local application - you'd want to convert your project's metadata to a Nix expression and let Nix handle the dependency management.
- Overlay adds/overrides something in the global package set.
- In general, you should only install things with nix, and not use any other package managers.
- The main idea of the Nix approach is to store software components in isolation from each other in a central component store, under path names that contain cryptographic hashes of all inputs involved in building the component, such as `/nix/store/rwmfbhb2znwp...-ﬁrefox1.0.4`.
- Don't install libraries with Nix.
- Derivations are variables + build script.
- Docs are in `.xml` files in `/docs`
- To build derivation in nixpkgs, at root of `nixpkgs`, run `nix build -f . <pkg-name>` (ie `nix build -f . wifi-password`)
- To find out the SHA256, run `nix-prefetch-url -A <pkg-name>.src` (ie `nix-prefetch-url -A wifi-password.src`). `nix-prefetch-url` works with GitHub. This gives you the SHA256 you can copy.
- [I think Nix's approach is the right way to build container images--build the image layers declaratively, reproducibly, incrementally (no, Dockerfile builds aren't incremental because dependency trees aren't linear), and without a container runtime dependency.](https://twitter.com/weberc2/status/1334927112550174721)
- [Nix doesn't solve dependency resolution problems. Only pinning. There's ground to break there. Dependency management not being part of flakes is my biggest gripe with it. It would be our chance to be the once size fits all solution but we failed to deliver.](https://twitter.com/ProgrammerDude/status/1375451276234928132)
- [There's a lot of unexplored potential of Nix in granular build systems and displacing systems like Bazel. If applied correctly, it lets smaller organisations get much of the benefit of Google-style monorepos but without as much maintenance overhead.](https://news.ycombinator.com/item?id=26748696)
- [Nix doesn't have an issue with metadata revisions: they're just another explicitly declared input to the build so they don't violate purity.](https://twitter.com/GabriellaG439/status/1462150287871787009)
- [Nix has an incredibly steep learning curve atm, but has completely eliminated entire classes of problems for me such as per-proj software deps and consistent environments, and has removed any fear of experimentation (since I can always rollback). Nothing else does this today.](https://twitter.com/mitchellh/status/1482104113819045892)
- [Nixpkgs overlays are monoids, where pkgs.lib.composeExtensions is the associative binary operation and (_: _: { }) is the identity element.](https://twitter.com/GabriellaG439/status/1486009172365770754)

## Code

```bash
# Build nix package locally.

# cd into cloned https://github.com/NixOS/nixpkgs

# Build package from default.nix inside nixpkgs. Will put result as ./result if succeeds
# i.e. nix-build -A watchexec -> will build watchexec package
nix-build -A <package>

# Install the build and put it `~/.nix-profile/bin`
nix-env -i ./result
```

```bash
# Garbage collect
sudo nix-collect-garbage --delete-older-than 30d
```

## Links

- [Nix Manual](https://nixos.org/manual/nix/stable/)
- [Nix Pills](https://nixos.org/guides/nix-pills/) ([Code](https://github.com/NixOS/nix-pills))
- [Benefits of using nix](https://www.reddit.com/r/haskell/comments/7wmhyi/an_opinionated_guide_to_haskell_in_2018/du2506q/)
- [Nix, the purely functional build system](http://www.boronine.com/2018/02/02/Nix/) - Great intro article.
- [A Gentle Introduction to the Nix Family](https://ebzzry.io/en/nix/)
- [Nix: A Reproducible Setup for Linux and macOS](http://nmattia.com/posts/2018-03-21-nix-reproducible-setup-linux-macos.html)
- [hnix](https://github.com/jwiegley/hnix) - Haskell re-implementation of the Nix expression language.
- [Haskell & Nix](https://github.com/Gabriel439/haskell-nix)
- [Brian McKenna - Nix for Functional Systems](https://www.youtube.com/watch?v=mIxtBVKo7JE)
- [Learning Nix by Example: Building FFmpeg 4.0](https://blog.kiloreux.me/2018/05/24/learning-nix-by-example-building-ffmpeg-4-dot-0/)
- [nix-shell and Shebang Lines](http://iam.travishartwell.net/2015/06/17/nix-shell-shebang/)
- [Domen Kožar, Lead DevOps, Nix workshop](https://www.youtube.com/watch?v=BjRGlKNHeEc)
- [Cheap Docker images with Nix](http://lethalman.blogspot.com/2016/04/cheap-docker-images-with-nix_15.html)
- [When to use declarative approach and when not](https://www.reddit.com/r/NixOS/comments/95vczu/when_to_use_declarative_approach_and_when_not/)
- [example-nix](https://github.com/shajra/example-nix) - A way to develop software with Nix.
- [Hercules CI](https://hercules-ci.com/) - Hosted CI for building Nix projects on your infrastructure.
- [Dysnomia](https://github.com/svanderburg/dysnomia) - Tool and plug-in system that can be used to automatically deploy mutable components.
- [Disnix](https://github.com/svanderburg/disnix) - Nix-based distributed service deployment tool.
- [NUR](https://github.com/nix-community/NUR) - Nix User Repository: User contributed nix packages.
- [Eris](https://github.com/thoughtpolice/eris) - Binary cache for Nix.
- [pypi2nix](https://github.com/garbas/pypi2nix) - Generate Nix expressions for Python packages.
- [hnix-store](https://github.com/haskell-nix/hnix-store) - Haskell implementation of the nix store API.
- [nix-cheatsheet](https://github.com/knedlsepp/nix-cheatsheet)
- [Nix RFCs](https://github.com/NixOS/rfcs)
- [nix-linter](https://github.com/Synthetica9/nix-linter) - Linter for the Nix expression language.
- [Install Nix docs by Mozilla](https://docs.mozilla-releng.net/develop/install-nix.html) - Pretty good.
- [Nix scripts shared across IOHK projects](https://github.com/input-output-hk/iohk-nix)
- [niv](https://github.com/nmattia/niv) - Painless dependencies for Nix projects.
- [Cachix](https://cachix.org/) - Build Nix packages once and share them for good. ([Docs](https://docs.cachix.org/)) ([Docs Code](https://github.com/cachix/docs.cachix.org))
- [Alternative Haskell Infrastructure for Nixpkgs](https://github.com/input-output-hk/haskell.nix) - Works by automatically translating your Cabal or Stack project and its dependencies into Nix code.
- [nix-bundle](https://github.com/matthewbauer/nix-bundle) - Bundle Nix derivations to run anywhere.
- [crate2nix](https://github.com/kolloch/crate2nix) - Nix build file generator for rust crates. ([Lobsters](https://lobste.rs/s/26xnzy/crate2nix_nix_build_file_generator_for))
- [lorri](https://github.com/nix-community/lorri) - nix-shell replacement for project development.
- [Awesome Nix](https://github.com/nix-community/awesome-nix)
- [nixfmt](https://github.com/serokell/nixfmt) - Formatter for Nix code.
- [Nix for devs](https://github.com/uniphil/nix-for-devs) - Collection of recipes focused on nix-shell to make setting up project environments easy.
- [nixpkgs-fmt](https://github.com/nix-community/nixpkgs-fmt) - Nix code formatter for nixpkgs. ([Web](https://nix-community.github.io/nixpkgs-fmt/))
- [Nix builder for Kubernetes](https://gist.github.com/tazjin/08f3d37073b3590aacac424303e6f745)
- [Nixery](https://nixery.dev/) - Provides the ability to pull ad-hoc container images from a Docker-compatible registry server. ([Code](https://github.com/tazjin/nixery)) ([Talk](https://www.youtube.com/watch?v=pOI9H4oeXqA)) ([HN](https://news.ycombinator.com/item?id=31079144))
- [Nixery: Improved Layering Design (2019)](https://tazj.in/blog/nixery-layers)
- [hnix-lsp](https://github.com/domenkozar/hnix-lsp) - Language Server Protocol for Nix.
- [Make Nix precisely emulate gitignore](https://github.com/hercules-ci/gitignore.nix)
- [Nixery](https://github.com/google/nixery) - Container registry which transparently builds images using the Nix package manager.
- [wharfix](https://github.com/google/nixery) - Minimal stateless+readonly docker registry based on nix expressions. Heavily inspired by Nixery.
- [Nix - A One Pager](https://github.com/tazjin/nix-1p) - A (more or less) one page introduction to Nix, the language.
- [yants](https://code.tvl.fyi/about/nix/yants) - Tiny type-checker for data in Nix, written in Nix. ([HN](https://news.ycombinator.com/item?id=30395667))
- [nix-shorts](https://github.com/justinwoo/nix-shorts) - Collection of short notes about Nix, down to what is immediately needed for users.
- [rnix-parser](https://github.com/nix-community/rnix-parser) - Nix parser written in Rust.
- [Naersk](https://github.com/nmattia/naersk) - Nix support for building cargo crates.
- [nix-diff](https://github.com/Gabriel439/nix-diff) - Explain why two Nix derivations differ.
- [Optimising Docker Layers for Better Caching with Nix (2018)](https://grahamc.com/blog/nix-and-layered-docker-images)
- [nix-du](https://github.com/symphorien/nix-du) - Visualise which gc-roots to delete to free some space in your nix store.
- [NixCon 2019 - Main Track](https://www.youtube.com/watch?v=aUG9aGYYCY8)
- [Nix flakes (2019)](https://www.youtube.com/watch?v=UeBX7Ide5a0)
- [Nix: How and Why it Works (2019)](https://www.youtube.com/watch?v=lxtHH838yko)
- [Nix recipes for Haskellers](https://www.srid.ca/haskell-nix.html)
- [format-nix](https://github.com/justinwoo/format-nix) - Simple formatter for Nix using tree-sitter-nix.
- [go-nix](https://github.com/orivej/go-nix) - Nix language parser and Nix-compatible file hasher in Go.
- [nix-dns](https://github.com/kirelagin/nix-dns) - Nix DSL for DNS zone files.
- [Nix-based app VMs](https://github.com/jollheef/appvm)
- [nix-index](https://github.com/bennofs/nix-index) - Quickly locate nix packages with specific files.
- [Nix-bisect](https://github.com/timokau/nix-bisect) - Bisect Nix Builds.
- [Thoughts on Nix (2020)](https://christine.website/blog/thoughts-on-nix-2020-01-28) ([Lobsters](https://lobste.rs/s/tp6o0q/thoughts_on_nix))
- [I was Wrong about Nix (2020)](https://christine.website/blog/i-was-wrong-about-nix-2020-02-10) ([HN](https://news.ycombinator.com/item?id=22295102)) ([Lobsters](https://lobste.rs/s/4otqzp/i_was_wrong_about_nix))
- [Grafanix](https://github.com/stolyaroleh/grafanix) - Visualize your Nix dependencies.
- [What's your configuration.nix like?](https://www.reddit.com/r/NixOS/comments/9aa08b/whats_your_configurationnix_like/)
- [Built with Nix](https://builtwithnix.org/) - Build software only once. ([Code](https://github.com/nix-community/builtwithnix.org))
- [How I Start: Nix (2020)](https://christine.website/blog/how-i-start-nix-2020-03-08) ([Lobsters](https://lobste.rs/s/lktf6u/how_i_start_nix))
- [rnix-lsp](https://github.com/nix-community/rnix-lsp) - WIP Language Server for Nix.
- [Language server for Nix language](https://github.com/ebkalderon/nix-language-server)
- [Eelco Dolstra's talks/papers](https://edolstra.github.io/) ([Code](https://github.com/edolstra/edolstra.github.io))
- [Nix Haskell Monorepo Tutorial](https://github.com/fghibellini/nix-haskell-monorepo)
- [The journey of packaging a .NET app on Nix (2020)](https://sgt.hootr.club/molten-matter/dotnet-on-nix/)
- [nixpkgs](https://github.com/NixOS/nixpkgs) - Nix Packages collection.
- [Nix IRC logs](https://logs.nix.samueldr.com/)
- [Nixology (2020)](https://www.youtube.com/playlist?list=PLRGI9KQ3_HP_OFRG6R-p4iFgMSK1t5BHs) - Series of videos I've been releasing within Shopify to help promote and educate about Nix.
- [Nix function to easily create derivations (packages) to install binaries from location](https://twitter.com/mitchellh/status/1259521715211657216)
- [What Is Nix (2020)](https://engineering.shopify.com/blogs/engineering/what-is-nix) ([HN](https://news.ycombinator.com/item?id=23251754)) ([Lobsters](https://lobste.rs/s/bgwsd8/what_is_nix))
- [What Is Nix and Why You Should Use It (2020)](https://serokell.io/blog/what-is-nix)
- [comma](https://github.com/Shopify/comma) - Runs software without installing it. Wraps together nix run and nix-index.
- [nix-derivation](https://github.com/Gabriel439/Haskell-Nix-Derivation-Library) - Parse and render \*.drv files.
- [nix-build-uncached](https://github.com/Mic92/nix-build-uncached) - CI friendly wrapper around nix-build.
- [nix-tests](https://github.com/cleverca22/nix-tests) - Scratchpad for small experimental things I am doing with Nix.
- [Nix Flakes, Part 1: An introduction and tutorial (2020)](https://www.tweag.io/posts/2020-05-25-flakes.html)
- [Nix Flakes, Part 1: An introduction and tutorial](https://www.tweag.io/blog/2020-05-25-flakes/) ([Lobsters](https://lobste.rs/s/h99uo8/nix_flakes_part_1_introduction_tutorial)) ([Lobsters 2](https://lobste.rs/s/iqoegm/nix_flakes_part_1_introduction_tutorial))
- [nix-overlays of Antonio Monteiro](https://github.com/anmonteiro/nix-overlays)
- [A Nix terminology primer by a newcomer (2020)](https://stephank.nl/p/2020-06-01-a-nix-primer-by-a-newcomer.html)
- [Statistical Rethinking and Nix (2020)](https://rgoswami.me/posts/rethinking-r-nix/)
- [flake-utils](https://github.com/numtide/flake-utils) - Pure Nix flake utility functions.
- [nix.dev](https://nix.dev/) - Opinionated guide for developers wanting to get things done using the Nix ecosystem. ([Code](https://github.com/nix-dot-dev/nix.dev))
- [Nix language antipatterns](https://nix.dev/anti-patterns/language.html)
- [So, tell me about Nix (2020)](https://ghedam.at/15490/so-tell-me-about-nix)
- [nixdu](https://github.com/utdemir/nixdu) - Interactively browse the dependency graph of your Nix derivations.
- [Nix Package Versions](https://lazamar.co.uk/nix-versions/) - Search for old versions of Nix packages. ([Code](https://github.com/lazamar/nix-package-versions)) ([Reddit](https://www.reddit.com/r/NixOS/comments/gc68ec/find_older_versions_of_a_package_in_the_nix/))
- [Opinionated Nix repository template](https://github.com/nix-dot-dev/getting-started-nix-template) - Based on nix.dev tutorials, repository template to get you started with Nix.
- [Tools to manage a Nix-based project](https://github.com/shajra/nix-project)
- [Building static Haskell binary with Nix on Linux (2020)](https://blog.patchgirl.io/haskell/2020/07/13/static-haskell-binary.html)
- [Template for NUR repositories](https://github.com/rvolosatovs/nur-packages)
- [Bramble](https://github.com/maxmcd/bramble) - Functional build system inspired by nix. ([Article](https://maxmcd.com/posts/bramble/)) ([Lobsters](https://lobste.rs/s/g1tqfe/bramble_purely_functional_build_system))
- [The easiest way (I've found) to create your own Nix channel (2020)](https://lucperkins.dev/blog/nix-channel/)
- [Nix Monorepo](https://github.com/lucperkins/nix-monorepo) - How you might use Nix in a larger, multi-language project.
- [A Tutorial Introduction to Nix (2020)](https://rgoswami.me/posts/ccon-tut-nix/)
- [nixbuild.net](https://nixbuild.net/) - Cloud service that runs your Nix builds. It takes away the effort of maintaining and scaling build clusters, and integrates easily with any setup that uses Nix. ([Docs](https://docs.nixbuild.net/)) ([Tweet](https://twitter.com/utdemir/status/1299850167546380288))
- [Manix](https://github.com/mlvzk/manix) - Fast CLI documentation searcher for Nix.
- [Review of home manager (2020)](https://magnusson.io/post/home-manager-review/) ([Lobsters](https://lobste.rs/s/pys2ta/review_home_manager))
- [Nix Quick Install Action](https://github.com/nixbuild/nix-quick-install-action) - GitHub Action installs Nix in single-user mode, and adds almost no time at all to your workflow's running time. ([Web](https://github.com/marketplace/actions/nix-quick-install))
- [Setting up a Nix S3 binary cache (2020)](https://fzakaria.com/2020/07/15/setting-up-a-nix-s3-binary-cache.html) ([Lobsters](https://lobste.rs/s/myps2p/setting_up_nix_s3_binary_cache))
- [swift2nix: Run Swift inside Nix builds (2020)](https://euandre.org/2020/10/05/swift2nix-run-swift-inside-nix-builds.html)
- [sorri](https://github.com/nmattia/sorri) - Simple, lightweight implementation of Tweag's lorri.
- [Nix and the nix-shell for easily redistributable scripts (2020)](https://knezevic.ch/posts/nix-shell-redistributable-scripts/)
- [nix-buildkite](https://github.com/circuithub/nix-buildkite) - Take a Nix job description and turn it into separate Buildkite steps with dependencies. ([Tweet](https://twitter.com/acid2/status/1314507071224676352))
- [pre-commit-hooks.nix](https://github.com/cachix/pre-commit-hooks.nix) - Seamless integration of pre-commit git hooks with Nix.
- [Nix + Haskell setup](https://twitter.com/acid2/status/1314507569541640192)
- [caching your nix-shell (2020)](https://fzakaria.com/2020/08/11/caching-your-nix-shell.html)
- [nix-direnv](https://github.com/nix-community/nix-direnv) - Fast, persistent use_nix implementation for direnv.
- [NixCon 2020](https://2020.nixcon.org/) ([Stream](https://www.youtube.com/watch?v=7sQa04olUA0)) ([Code](https://github.com/nixcon/2020.nixcon.org)) ([HN](https://news.ycombinator.com/item?id=24799659))
- [Nix UX improvements](https://github.com/tweag/nix-ux)
- [NixCon 2020 talk about Nix flakes](https://github.com/serokell/nixcon2020-talk)
- [Nickel](https://github.com/tweag/nickel) - Lightweight configuration language. Its purpose is to automate the generation of static configuration files. ([Nickel: better configuration for less](https://www.tweag.io/blog/2020-10-22-nickel-open-sourcing/)) ([First release of Nickel](https://www.tweag.io/blog/2022-03-11-nickel-first-release/)) ([Lobsters](https://lobste.rs/s/hskr5v/first_release_nickel))
- [Nix-based process management framework](https://github.com/svanderburg/nix-processmgmt)
- [Local Nix Cache](https://github.com/andir/local-nix-cache) - Poor and hacky attempt at re-serving local nix packages that came from trusted sources.
- [Nix parallelism & Import From Derivation (2020)](https://fzakaria.com/2020/10/20/nix-parallelism-import-from-derivation.html) ([Reddit](https://www.reddit.com/r/NixOS/comments/jf79zy/nix_parallelism_import_from_derivation/))
- [macOS Nix setup: an alternative to Homebrew (2020)](https://wickedchicken.github.io/post/macos-nix-setup/) ([Lobsters](https://lobste.rs/s/rtluby/macos_nix_setup_alternative_homebrew)) ([HN](https://news.ycombinator.com/item?id=27825240))
- [update-nix-fetchgit](https://github.com/expipiplus1/update-nix-fetchgit) - Program to automatically update fetchgit values in Nix expressions.
- [Cache install Nix packages](https://github.com/nix-actions/cache-install) - Use the GitHub Actions cache for Nix packages.
- [nixbuild.net Action](https://github.com/nixbuild/nixbuild-action) - GitHub Action for using the nixbuild.net service.
- [fromElisp](https://github.com/talyz/fromElisp) - Emacs Lisp reader in Nix.
- [On-demand linked libraries for Nix (2020)](https://fzakaria.com/2020/11/17/on-demand-linked-libraries-for-nix.html) ([Lobsters](https://lobste.rs/s/nawo6d/on_demand_linked_libraries_for_nix))
- [Towards a Content-Addressed Model for Nix (2020)](https://www.tweag.io/blog/2020-09-10-nix-cas/) ([HN](https://news.ycombinator.com/item?id=27070858))
- [deploy-rs](https://github.com/serokell/deploy-rs) - Simple, multi-profile Nix-flake deploy tool.
- [TodoMVC-Nix](https://github.com/nix-community/todomvc-nix) - One-stop reference to build TodoMVC application inside the Nix world.
- [Trustix: Distributed trust and reproducibility tracking for binary caches (2020)](https://www.tweag.io/blog/2020-12-16-trustix-announcement/) ([Code](https://github.com/tweag/trustix))
- [Trustix - Consensus and voting (2022)](https://www.tweag.io/blog/2022-02-03-trustix-voting/)
- [Binary Verification with Trustix starring Adam Höse (2021)](https://overcast.fm/+i6QEhiFYA)
- [Building with Nix Flakes for, eh .. reasons (2021)](https://blog.ysndr.de/posts/internals/2021-01-01-flake-ification/)
- [What is the right approach to handling binary, non-executable data dependencies of packages? (2021)](https://www.reddit.com/r/NixOS/comments/kqe57g/what_is_the_right_approach_to_handling_binary/)
- [Scrive Nix Workshop](https://scrive.github.io/nix-workshop/) ([Code](https://github.com/scrive/nix-workshop))
- [nix-npm-buildpackage](https://github.com/serokell/nix-npm-buildpackage) - Build nix packages that use npm/yarn.
- [dev-env](https://github.com/digital-asset/dev-env) - Nix with training wheels.
- [Basinix](https://github.com/jonringer/basinix) - Pull request reviewing tool for nixpkgs.
- [Nix-template](https://github.com/jonringer/nix-template) - Make creating nix expressions easy. Provide a nice way to create largely boilerplate nix-expressions.
- [nixpkgs-hammering](https://github.com/jtojnar/nixpkgs-hammering) - Set of nit-picky rules that aim to point out and explain common mistakes in nixpkgs package pull requests.
- [nix-script](https://github.com/BrianHicks/nix-script) - Write scripts in compiled languages that run in the nix ecosystem, with no separate build step. ([Article](https://bytes.zone/posts/nix-script/))
- [nix-update](https://github.com/Mic92/nix-update) - Updates versions/source hashes of nix packages. It is designed to work with nixpkgs but also other package sets.
- [Nix Portable](https://github.com/DavHau/nix-portable) - Static, Permissionless, Installation-free, Pre-configured.
- [nix-optics](https://github.com/masaeedu/nix-optics) - Using profunctor optics to focus modifications in Nix.
- [Use Nix flakes without any fluff](https://github.com/gytis-ivaskevicius/flake-utils-plus)
- [Nix-environments](https://github.com/nix-community/nix-environments) - Repository to maintain out-of-tree shell.nix files.
- [Determinate Systems](https://determinate.systems/) - Confidently build and deploy to the cloud, stadium, or stock exchange. Expert help with the Nix ecosystem from Graham. ([Twitter](https://twitter.com/DeterminateSys)) ([GitHub](https://github.com/DeterminateSystems))
- [How to Learn Nix](https://ianthehenry.com/posts/how-to-learn-nix/) ([HN](https://news.ycombinator.com/item?id=29303641))
- [Nix is the ultimate DevOps toolkit (2021)](https://tech.channable.com/posts/2021-04-09-nix-is-the-ultimate-devops-toolkit.html) ([HN](https://news.ycombinator.com/item?id=26748696)) ([Lobsters](https://lobste.rs/s/5gbbp2/nix_is_ultimate_devops_toolkit))
- [devshell](https://github.com/numtide/devshell) - Per project developer environments.
- [Nix Data](https://github.com/nix-community/nix-data) - Set of packages for data-scientists with batteries-included.
- [Nomia](https://github.com/scarf-sh/nomia) - System for precise, efficient resource management across every domain and resource type.
- [Chimera](https://github.com/craftweg/chimera) - Nix-based package manager with the focus on developer experience.
- [Data Science with Nix: Parameter Sweeps (2021)](https://blog.nixbuild.net/posts/2021-04-26-data-science-with-nix-parameter-sweeps.html)
- [Nixpkgs rules for Bazel](https://github.com/tweag/rules_nixpkgs) - Rules for importing Nixpkgs packages into Bazel.
- [Practical Nix Flakes (2021)](https://serokell.io/blog/practical-nix-flakes)
- [npmlock2nix](https://github.com/tweag/npmlock2nix) - Utilizing npm lockfiles to create Nix expressions for NPM based projects.
- [Laurn](https://github.com/baloo/laurn) - Run a dev-environment in a pure-ish nix environment.
- [nix-filter](https://github.com/numtide/nix-filter) - Small self-container source filtering lib.
- [nix-eval-lsp](https://github.com/aaronjanse/nix-eval-lsp) - Nix language server that evaluates code.
- [Flakes are such an obviously good thing: ...but the design and development process should be better (2021)](https://grahamc.com/blog/flakes-are-an-obviously-good-thing) ([Lobsters](https://lobste.rs/s/ipvgon/flakes_are_such_obviously_good_thing))
- [scratchix](https://github.com/dramforever/scratchix) - Linux From Scratch, but it's Nix.
- [Understanding Nix's String Context (2018)](https://shealevy.com/blog/2018/08/05/understanding-nixs-string-context/)
- [Replit now supports every programming language in Nix (2021)](https://blog.replit.com/nix) ([HN](https://news.ycombinator.com/item?id=27269292))
- [nix-graph](https://github.com/awakesecurity/nix-graph) - Reify the Nix build graph into a Haskell graph data structure.
- [Digga](https://github.com/divnix/digga) - Feature rich and configurable framework for harnessing Nix Flakes.
- [Nix solves the package manager ejection problem (2021)](https://zeroindexed.com/nix-ejection-problem) ([HN](https://news.ycombinator.com/item?id=27344677))
- [Cross compilation using Nix](https://nix.dev/tutorials/cross-compilation) ([Tweet](https://twitter.com/burdiyan/status/1448778093188030464))
- [nix-std](https://github.com/chessai/nix-std) - no-nixpkgs standard library for the nix expression language.
- [Nix unstable installer](https://github.com/numtide/nix-unstable-installer) - Place to host Nix unstable releases.
- [Nix Learning](https://github.com/humancalico/nix-learning) - Links to blog posts, articles, videos, etc for learning Nix.
- [nix-plugins](https://github.com/shlevy/nix-plugins) - Collection of useful Nix native plugins.
- [Nix.Ci](https://nix.ci/) - Provides the CI integration infrastructure for Nixpkgs and NixOS. More commonly known as OfBorg. ([Code](https://github.com/NixOS/ofborg))
- [nix-user-chroot](https://github.com/nix-community/nix-user-chroot) - Install & Run nix without root permissions.
- [Another Nix Success Story (2021)](https://maxdeviant.com/shards/2021/another-nix-success-story/) ([Lobsters](https://lobste.rs/s/sfacek/another_nix_success_story))
- [nix bundle](https://nixos.org/manual/nix/unstable/command-ref/new-cli/nix3-bundle.html) - Bundle an application so that it works outside of the Nix store.
- [Building Container Images with Nix (2021)](https://thewagner.net/blog/2021/02/25/building-container-images-with-nix/) ([HN](https://news.ycombinator.com/item?id=28240748))
- [Debug symbols for binaries with Nix (2021)](http://rski.github.io/2021/09/05/nix-debugging.html)
- [Ditch your version manager and use Nix (2021)](https://juliu.is/ditch-your-version-manager/) ([Lobsters](https://lobste.rs/s/emyfhx/ditch_your_version_manager)) ([HN](https://news.ycombinator.com/item?id=28565072))
- [Advanced shell packaging: resholve YADM's nixpkg (2021)](https://t-ravis.com/post/nix/advanced_shell_packaging_resholve_yadm/)
- [nix-parsec](https://github.com/nprindle/nix-parsec) - Parser combinators in Nix.
- [nix-prefetch-github](https://github.com/seppeljordan/nix-prefetch-github) - Prefetch sources from github for nix build tool.
- [Building reproducible Development environment with nix-shell (2021)](https://mudrii.medium.com/building-reproducible-development-environment-b1d4fb51a302)
- [Building with Nix on Replit](https://docs.replit.com/tutorials/30-build-with-nix) ([HN](https://news.ycombinator.com/item?id=28733156))
- [dream2nix](https://github.com/DavHau/dream2nix) - Generic framework for 2nix converters (converting from other build systems to nix).
- [Novice Nix: Flake Templates (2021)](https://peppe.rs/posts/novice_nix:_flake_templates/)
- [Personal provisioning machines with Nix](https://github.com/shajra/shajra-provisioning)
- [Issues you encountered learning Nix? (2021)](https://twitter.com/theprincessxena/status/1448984266071752721)
- [Declarative Dev Environments using Nix (2021)](https://marcopolo.io/code/declarative-dev-environments/)
- [Peerix](https://github.com/cid-chan/peerix) - Peer-to-peer binary cache for nix derivations.
- [update-flake-lock](https://github.com/DeterminateSystems/update-flake-lock) - GitHub Action that will update your flake.lock file whenever it is run.
- [Replit - Faster Nix Repl Startup (2021)](https://blog.replit.com/nix-perf-improvements)
- [Statix](https://github.com/nerdypepper/statix) - Lints and suggestions for the nix programming language. ([Reddit](https://www.reddit.com/r/NixOS/comments/qh47fz/statix_lints_and_suggestions_for_the_nix/))
- [Cicero](https://github.com/input-output-hk/cicero) - Workflow execution engine. Workflow is a description of (dependent) steps using the Nix configuration language.
- [Cross-compiling and Static-linking with Nix (2021)](https://functor.tokyo/blog/2021-10-20-nix-cross-static)
- [Nix 2.4 (2021)](https://discourse.nixos.org/t/nix-2-4-released/15822) ([Lobsters](https://lobste.rs/s/cumwee/nix_2_4_released)) ([HN](https://news.ycombinator.com/item?id=29073301))
- [nixcrpkgs](https://github.com/pololu/nixcrpkgs) - Tools for cross-compiling standalone applications using Nix.
- [Flake structure](https://github.com/freezeboy/flake-example)
- [nix-patchtools](https://github.com/svanderburg/nix-patchtools) - Autopatching binary packages to make them work with Nix.
- [A Critique of Nix Package Manager](https://www.iohannes.us/en/commentary/nix-critique/) ([Reddit](https://www.reddit.com/r/NixOS/comments/qs529l/a_critique_of_nix_package_manager/))
- [rusty-nix](https://github.com/Kloenk/rusty-nix) - Nix written in rust.
- [Some Nix questions (2021)](https://www.reddit.com/r/NixOS/comments/qsfkow/new_to_nix_some_questions/)
- [Nix - Flakes for out-of-tree code (2021)](https://www.youtube.com/watch?v=90P-Ml1318U)
- [Is it worth learning and migrating to Flakes as of November 2021?](https://www.reddit.com/r/NixOS/comments/qv9ag1/is_it_worth_learning_and_migrating_to_flakes_as/)
- [dreampkgs](https://github.com/nix-community/dreampkgs) - Collection of software packages managed with dream2nix, a framework for automated packaging.
- [Flake templates](https://github.com/NixOS/templates)
- [libnix](https://github.com/Profpatsch/libnix-haskell) - Haskell library to interface with the nix package manager.
- [nix-exec](https://github.com/shlevy/nix-exec) - Run programs defined in nix expressions.
- [Closure vs. derivation in the Nix package manager (2018)](https://medium.com/scientific-breakthrough-of-the-afternoon/closure-vs-derivation-in-the-nix-package-manager-ec0eccc53407)
- [mk-darwin-system](https://github.com/vic/mk-darwin-system) - Small Nix utility to create an M1 aarch64-darwin (nixFlakes + nix-darwin + home-manager) system.
- [Nix language for JavaScript developers](https://github.com/rofrol/nix-for-javascript-developers)
- [Will Nix Overtake Docker (2021)](https://blog.replit.com/nix-vs-docker) ([HN](https://news.ycombinator.com/item?id=29387137)) ([Lobsters](https://lobste.rs/s/65xymz/will_nix_overtake_docker))
- [One too many shell (2021)](https://blog.ysndr.de/posts/guides/2021-12-01-nix-shells/)
- [Building a reproducible blog with Nix (2020)](https://blog.ysndr.de/posts/internals/2020-04-10-built-with-nix/)
- [Tvix: We Are Rewriting Nix (2021)](https://tvl.fyi/blog/rewriting-nix) ([HN](https://news.ycombinator.com/item?id=29412971)) ([Lobsters](https://lobste.rs/s/ypwgwp/tvix_we_are_rewriting_nix))
- [Implementing a content-addressed Nix (2021)](https://www.tweag.io/blog/2021-12-02-nix-cas-4/)
- [BuildKit-Nix](https://github.com/AkihiroSuda/buildkit-nix) - Allows using Nix derivations (default.nix) as Dockerfiles.
- [How Nix and NixOS Get So Close to Perfect (2021)](https://www.youtube.com/watch?v=qjq2wVEpSsA)
- [deadnix](https://github.com/astro/deadnix) - Scan Nix files for dead code.
- [nix-casync](https://github.com/flokli/nix-casync) - More efficient way to store and substitute Nix store paths. ([Article](https://flokli.de/posts/2021-12-10-nix-casync-intro/)) ([Tweet](https://twitter.com/flokli/status/1469310495756898311))
- [Untrusted CI: Using Nix to get automatic trusted caching of untrusted builds (2019)](https://flokli.de/posts/2019-11-21-untrusted-ci/)
- [nix-universal-prefetch](https://github.com/samueldr/nix-universal-prefetch) - Uses nix and nixpkgs to actually run the prefetch operation, then read the error message to figure out the desired hash.
- [Nix Starter Config](https://github.com/Misterio77/nix-starter-config) - Simple config repo to get you started with NixOS + home-manager + flakes.
- [Compatibility function to allow flakes to be used by non-flake-enabled Nix versions](https://github.com/edolstra/flake-compat)
- [Tools You Should Know About: nix-shell (2021)](https://cuddly-octo-palm-tree.com/posts/2021-12-19-tyska-nix-shell/) ([Lobsters](https://lobste.rs/s/4khflt/tools_you_should_know_about_nix_shell))
- [Basic implementation of a Nix-like store abstraction over a Merkle tree](https://github.com/ebkalderon/merkle-tree-nix-store-thing)
- [tower-lsp](https://github.com/ebkalderon/tower-lsp) - Language Server Protocol implementation written in Rust.
- [direnv-nix-lorelei](https://github.com/shajra/direnv-nix-lorelei) - Alternative Nix extension of Direnv.
- [nix-eval-jobs](https://github.com/nix-community/nix-eval-jobs) - Parallel nix evaluator with a streamable JSON output.
- [go-nix](https://github.com/numtide/go-nix) - Nix experiments written in go.
- [Nixpkgs Overlays Are Monoids](https://www.haskellforall.com/2022/01/nixpkgs-overlays-are-monoids.html) ([HN](https://news.ycombinator.com/item?id=30090183))
- [Building fully declarative systems with Nix (2022)](https://overcast.fm/+vE2fezc98)
- [nix2container](https://github.com/nlewo/nix2container) - Archive-less dockerTools.buildImage implementaion.
- [Hydra](https://github.com/NixOS/hydra) - Nix-based continuous build system. ([CLI](https://github.com/nlewo/hydra-cli))
- [Exploring Nix Flakes: Usable Go Plugins (2022)](https://flyx.org/nix-flakes-go/)
- [gradle2nix](https://github.com/tadfisher/gradle2nix) - Generate Nix expressions which build Gradle-based projects.
- [npins](https://github.com/andir/npins) - Simple and convenient pinning of dependencies in Nix.
- [nix-docker-base](https://github.com/teamniteo/nix-docker-base) - Nix Docker base images for fast and minimal builds.
- [Nix: An Idea Whose Time Has Come (2022)](https://revelry.co/insights/development/nix-time/) ([Lobsters](https://lobste.rs/s/ui7wc4/nix_idea_whose_time_has_come)) ([HN](https://news.ycombinator.com/item?id=30384121))
- [Alejandra](https://github.com/kamadorueda/alejandra) - Uncompromising Nix Code Formatter.
- [Makes](https://github.com/fluidattacks/makes) - DevSecOps framework powered by Nix.
- [Standard](https://github.com/divnix/std) - Opinionated, generic, Nix Flakes framework that will allow you to grow and cultivate Nix Cells with ease.
- [nix-book](https://github.com/divnix/nix-book) - Nix Package Manager. ([Docs](https://book.divnix.com/))
- [Nix Flakes: an Introduction (2022)](https://christine.website/blog/nix-flakes-1-2022-02-21) ([Lobsters](https://lobste.rs/s/dmrnqy/nix_flakes_introduction)) ([Reddit](https://www.reddit.com/r/NixOS/comments/sxve1w/nix_flakes_an_introduction/)) ([HN](https://news.ycombinator.com/item?id=30416557))
- [All New Repls are Powered By Nix (2022)](https://blog.replit.com/powered-by-nix)
- [Nix Flakes: Packages and How to Use Them (2022)](https://christine.website/blog/nix-flakes-2-2022-02-27)
- [libnar](https://github.com/ebkalderon/libnar) - Library for reading and writing from NAR (Nix Archive) files written in Rust.
- [nix-output-monitor](https://github.com/maralorn/nix-output-monitor) - Pipe your nix-build output through the nix-output-monitor (aka nom) to get additional information while building.
- [nixpack](https://github.com/flatironinstitute/nixpack) - nix+spack.
- [nix-html](https://github.com/ursi/nix-html) - HTML DSL for Nix.
- [nickel-nix](https://github.com/nickel-lang/nickel-nix) - Experimental Nix toolkit to use nickel as a language for writing nix packages, shells and more.
- [What's new in Nix 2.7.0? (2022)](https://www.youtube.com/watch?v=TzQpmDrnRQA)
- [Nix Bundlers](https://github.com/NixOS/bundlers) - Way to transform derivations.
- [How to Learn Nix](https://ianthehenry.com/posts/how-to-learn-nix/introduction/) ([HN](https://news.ycombinator.com/item?id=30681182))
- [The hard part of type-checking Nix (2022)](https://www.haskellforall.com/2022/03/the-hard-part-of-type-checking-nix.html)
- [Shrinkwrap: Taming dynamic shared objects (2022)](https://fzakaria.com/2022/03/14/shrinkwrap-taming-dynamic-shared-objects.html)
- [any-nix-shell](https://github.com/haslersn/any-nix-shell) - Fish and Zsh support for the nix run and nix-shell environments of the Nix package manager.
- [Nix time (2022)](https://alexfedoseev.com/blog/post/nix-time)
- [Flake For Non-Nix Projects (2022)](https://duan.ca/2022/03/19/nix-dirnev/)
- [Shrinkwrap](https://github.com/fzakaria/shrinkwrap) - Tool that embosses the needed dependencies on the top level executable.
- [nix-dram](https://github.com/dramforever/nix-dram) - Nix Flakes with a modified frontend.
- [nixos-shell](https://github.com/chrisfarms/nixos-shell) - Tool for reproducable development environments described as NixOS modules.
- [nixpkgs-update](https://github.com/ryantm/nixpkgs-update) - Updating nixpkgs packages since 2018.
- [Awesome Nix high performance computing](https://github.com/freuk/awesome-nix-hpc)
- [Nix tutorials](https://github.com/mhwombat/nix-for-numbskulls)
- [Spongix](https://github.com/input-output-hk/spongix) - Proxy for Nix Caching.
- [Nixpacks](https://github.com/railwayapp/nixpacks) - App source + Nix packages + Docker = Image.
- [Summer of Nix](https://summer.nixos.org/)
- [Cache evaluation of nix functions](https://github.com/DavHau/nix-eval-cache)
- [nix-bundle-exe](https://github.com/3noch/nix-bundle-exe) - Simple Nix derivation to bundle Mach-O (macOS) and ELF (Linux) executables into a relocatable directory structure.
- [deploy-flake](https://github.com/antifuchs/deploy-flake) - Tool for deploying nix flakes to remote systems.
- [Nix Flakes: Exposing and using NixOS Modules (2022)](https://christine.website/blog/nix-flakes-3-2022-04-07)
- [NixEL](https://github.com/kamadorueda/nixel) - Lexer, Parser, Abstract Syntax Tree and Concrete Syntax Tree for the Nix Expressions Language.
- [Why nix is great as a build tool (2022)](https://news.ycombinator.com/item?id=30959774)
- [Nix grammar for tree-sitter](https://github.com/cstrahan/tree-sitter-nix)
- [Generating nix store paths with arbitrary prefixes](https://github.com/infinisil/nix-store-brute)
- [static-nix](https://github.com/jbedo/static-nix) - Statically linked version of Nix with some patches intended to ease the installation and usage of Nix on typical HPC clusters.