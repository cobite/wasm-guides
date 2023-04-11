First download juno

```markdown
git clone https://github.com/cosmoscontracts/juno
```

then go into the folder and run:

```markdown
make build && make install
```

Then this created the binary in a folder called bin.

Then copy this junod binary into

```markdown
/usr/local/bin
```

Then you should be able to use junod in the terminal.
You can try with 'junod'

If it does not work, you should ensure your .zshrc file contains the right configuration settings. This file is a hidden file in your home directory. You can view hidden files by holding 'cmd + shift + .' Here is mine as an example:

```markdown
export PATH="$HOME/.cargo/bin:$PATH"
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
```

Add this to your .zprofile:

```markdown
eval "$(/opt/homebrew/bin/brew shellenv)"
```

After updating, be sure to restart your terminal.

If you want to find the status of the locally running junod service:

```markdown
junod status
```

This should return a json object with status details.
