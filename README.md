# Nightly custom Emacs builds for macOS Nix environments
[![CI badge](https://github.com/zxfstd/gccemacs/actions/workflows/build.yaml/badge.svg)](https://github.com/zxfstd/gccemacs/actions/workflows/build.yaml)
[![BuiltWithNix badge](https://img.shields.io/badge/Built_With-Nix-5277C3.svg?logo=nixos&labelColor=73C3D5)](https://builtwithnix.org)
[![Cachix badge](https://img.shields.io/badge/Cachix-store-blue.svg?logo=hack-the-box&logoColor=73C3D5)](https://app.cachix.org/cache/gccemacs)
[![Emacs badge](https://img.shields.io/badge/Emacs-30.0.50-adadad.svg?logo=gnu-emacs&labelColor=9266CC&logoColor=ffffff)](http://git.savannah.gnu.org/cgit/emacs.git/log/)

This repository provides nightly automated builds of Emacs from HEAD for macOS Nix environments with the following additions:
- Native Compilation ([gccemacs](https://www.emacswiki.org/emacs/GccEmacs))
- [X Widgets](https://www.emacswiki.org/emacs/EmacsXWidgets) (Webkit) support
- [libvterm](https://github.com/akermu/emacs-libvterm)
- Patched window role to work nicely with [yabai](https://github.com/koekeishiya/yabai)

## Usage
To use this flake on your system, add it to your configuration inputs & overlays.  
It overlays the `pkgs.emacs` package.  
There is a [complimentary binary cache](https://app.cachix.org/cache/gccemacs) available which is pushed to nightly.
```nix
{
  inputs.darwin.url = "github:lnl7/nix-darwin";
  inputs.emacs.url = "github:zxfstd/gccemacs";

  outputs = { self, darwin, emacs }: {
    darwinConfigurations.example = darwin.lib.darwinSystem {
      modules = [
        {
          nix.binaryCaches = [
            "https://cachix.org/api/v1/cache/gccemacs"
          ];

          nix.binaryCachePublicKeys = [
            "gccemacs.cachix.org-1:ABLw5NpUyJ+AdeYBp3HultVA4NNXR1xiGEiTZCaa6/M="
          ];

          nixpkgs.overlays = [
            emacs.overlay
          ];
        }
      ];
    };
  };
}
```
