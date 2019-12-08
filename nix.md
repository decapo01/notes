
Make a dir for project
```
> mkdir my-proj
> cd my-proj
```

Run the nix cabal-install command

```
nix-shell --pure -p ghc cabal-install --run "cabal init"
```

Create a LICENSE file
```
> touch LICENSE
```

Create nix file from cabal
```
> nix-shell --pure -p cabal2nix --run "cabal2nix ." > default.nix
```

Create a release.nix file with the following content
```
let
  pkgs = import <nixpkgs> { };
in
  pkgs.haskellPackages.callPackage ./default.nix { }
```

