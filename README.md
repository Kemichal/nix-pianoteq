# Description

A nix flake that installs Pianoteq7 on NixOS. The license key and binary are still required and have to be acquired manually from [https://www.modartt.com/](https://www.modartt.com/).

# Usage

Right now, I am really new to NixOS and Nix so this might be an awkward way of installing a package. I might try to enhance this in the future and maybe even put it in the NUR.

1. Download the `pianoteq_linux_v841.7z` file from [https://www.modartt.com/](https://www.modartt.com/)
2. Put `pianoteq_linux_v841.7z` into the nix store and add a gcroot:
```sh
STORE_PATH=$(nix-store --add-fixed sha256 ./pianoteq_linux_v841.7z)
mkdir -p ~/.nix-gcroots
nix-store --realise --add-root ~/.nix-gcroots/pianoteq_linux_v841.7z --indirect $STORE_PATH
```

3. In your main `flake.nix` file add the following line to your inputs:

```nix
inputs = {
  #...
  pianoteq.url = "github:kemichal/nix-pianoteq/pianoteq8";
  #...
};
```

4. Use the following in order to install the package:

```nix
  environment.systemPackages = [
    #...
    pianoteq.packages.x86_64-linux.default
    #...
  ];
```

# Add a new Version

1. Download the new release from [https://www.modartt.com/](https://www.modartt.com/)

2. Get the new sha256 hash in sri format:

```sh
nix hash to-sri --type sha256 `sha256sum pianoteq_linux_$NEW_VERSION.7z`
```

3. Change the version numbers in the `flake.nix` file and adjust the sha256 hash:

4. Put the new release 7z into the nix store as documented under [Usage](#usage).
