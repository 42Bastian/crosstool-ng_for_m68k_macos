### Tutorial: Compile m68k-elf-gcc on MacOS

#### CrossTool-NG

##### Prerequisites
First of all install the required packages with brew running the following commands:

```
brew install gnu-sed
brew install binutils
brew install help2man
brew install gawk
brew install libtool
brew install ncurses
brew install autoconf
brew install automake
brew install bison
brew install diffutils
brew install flex
brew install git
brew install gperf
brew install make
brew install texinfo
brew install wget
brew install xz
brew install gmp
brew install mpfr
brew install openssl
brew install pcre
brew install readline
brew install bash
```

After that, open the file ".bash_profile" with the following command:

`nano $HOME/.bash_profile`

Add these lines at the end of the file:
(Replace "/opt/homebrew" by the respective path on your machine)

```
export PATH="/opt/homebrew/opt/libtool/libexec/gnubin:$PATH"
export PATH="/opt/homebrew/opt/ncurses/bin:$PATH"
export PATH="/opt/homebrew/opt/flex/bin:$PATH"
export PATH="/opt/homebrew/opt/openssl@3/bin:$PATH"
export PATH="/opt/homebrew/opt/binutils/bin:$PATH"

export LDFLAGS="-L/opt/homebrew/opt/binutils/lib -L/opt/homebrew/opt/ncurses/lib -L/opt/homebrew/opt/flex/lib -L/opt/homebrew/opt/openssl@3/lib"
export CPPFLAGS="-I/opt/homebrew/opt/binutils/include -I/opt/homebrew/opt/ncurses/include -I/opt/homebrew/opt/flex/include -I/opt/homebrew/opt/openssl@3/include"
```

Close the file with "CTRL+X" and press "Y". Reload your terminal running the following command:

`source $HOME/.bash_profile`

##### Downloading & Installing

Open a terminal to download, build and install crosstool-ng from source code.
Download crosstool-ng (release version 1.25.0)  with the the following command:

`wget https://github.com/crosstool-ng/crosstool-ng/releases/download/crosstool-ng-1.25.0/crosstool-ng-1.25.0.tar.xz`

Move into the downloaded folder and type the following commands:

```
tar -xvf crosstool-ng-1.25.0.tar.xz
cd crosstool-ng-1.25.0
./bootstrap
./configure --prefix=/usr/local
make
sudo make install
```

Check that crosstool-ng has been installed typing:

`ct-ng --version`

##### Preparing

Open the "Utility Disk" to make a new case sensitive volume.

Click on the "+" situated on the right corner of the window.
In my case I name it “crosstool-ng” which uses “APFS (case-sensitive)”. I've allocated 20GB about.

##### Making the cross-toolchain for Jaguar

Before to continue, close the terminal and delete/comment the lines added in .bash_profile then open again the terminal because we need to clear the old variable used to compile crosstool-ng. Then, move inside the volume created in the previous step:

`cd /Volumes/crosstool-ng`

Create a folder here called "src".

`mkdir src`

Copy the default config from this repo to `.config`

Then run

`ct-ng oldconfig`

You can now modify this config:

`ct-ng menuconfig`

Make sure that C++ is enabled in the section called "Addition supported language".

Now we can build the toolchain running:

`ct-ng build`

You might see an error from clang
```
[ERROR]    clang++: error: unsupported option '-print-multi-os-directory'
[ERROR]    clang++: error: no input files
```

But this (seems to be) not relevant.

Once complete, you have a folder `m68k-elf` with all the tools
