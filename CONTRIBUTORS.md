# setup

Using [overmind](https://github.com/DarthSim/overmind) to run things locally
```
brew install tmux overmind
```

# running
```
overmind start
```

Each process gets a labeled, color-coded output stream. A few useful Overmind commands while it's running:
```
overmind connect api   # attach to a specific process (e.g. to use the Flask debugger interactively)
overmind restart web   # restart just one process
overmind stop          # graceful shutdown of all
```
