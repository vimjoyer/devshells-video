# devshells-video

```nix
# flake.nix

{
  description = "test";

  inputs =
    {
      nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
      old-python-nixpkgs.url = "github:nixos/nixpkgs/2030abed5863fc11eccac0735f27a0828376c84e";
    };

  outputs = { self, nixpkgs, ... }@inputs:
    let
      system = "x86_64-linux";
      pkgs = nixpkgs.legacyPackages.${system};
    in
    {
      devShells.x86_64-linux.default =
        pkgs.mkShell
          {
            nativeBuildInputs = with pkgs; [
              nodejs
              inputs.old-python-nixpkgs.legacyPackages.${system}.python36
            ];
          };
    };
}

```

```nix
{ pkgs ? import <nixpkgs> { } }:

pkgs.mkShell
{
  nativeBuildInputs = with pkgs; [
    nodejs
  ];

  shellHook = ''
    echo "weclome"
    source ./something.sh
    echo "to my shell!" | ${pkgs.lolcat}/bin/lolcat 
  '';

  COLOR = "blue";

  PASSWORD = import ./password.nix;
}
```
