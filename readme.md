# Background:
Nix is great, but C++ tooling (expecially editor support), seems to be woefully inadequate. While the nix shell environment seamlessly exposes include paths to the compiler and build tooling, nix-shell falls flatwith IDE tooling. Tooling like language servers needs to know what the compiler knows. A few hacks have been created to patch this problem including:

1. [Mic92's ruby script](https://gist.github.com/Mic92/135e83803ed29162817fce4098dec144) for patching generated `compile_commands.json`.
2) [acowley's wrapper for ccls](https://github.com/acowley/dotfiles/blob/08689867cf7b6fc4a2752a7ac5206ac098f1867e/nix/ccls/nixAware.nix)
3)  acowley's nix-cquery shell script originally described in a [blog post](https://www.arcadianvisions.com/2018/nix-cquery.html)

# Rationale
I decided to create a derivation based on [acowley's cquery shell script hook](https://github.com/acowley/dotfiles/tree/master/nix/cquery), after reading through many threads on the Nixos Discourse in search of asolution to get IDE like features, like intellisense and linting, to work in a C++ environment. There was little or no consensus on how this should be done. Thankfully I stumbled on a nixos discourse thread whereacowley described why certain [C++ developer tools did not work without patching and hacks](https://discourse.nixos.org/t/c-ide-integration-autocomplete-etc/4021).

This helper function fills a gap in C++ tooling in nix, when working a nix-shell environment. It is a hack, but it is a transparent one, that does not depend on patching individual binaries. This helper scripr isprovided to help pass include directories Nix communicates in the environment and via wrapper scripts to a previously generated `compile_commands.json`. It augments the compiler command to explicitly include the headers from the nix environment variable.
