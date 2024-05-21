

## Llama2.c compilation on MonteCimone 

### Setup 

First, everyone has cloned Karpathy repository as follows:
```bash 
git clone https://github.com/karpathy/llama2.c.git
```
and entered to llama2 folder
```bash 
cd llama2.c
```
Then, here we forked our works, in particular each of us focused on one specific model (i worked with stories256K model, Lorenza with the one with 15M paramters, Luca with the 42M, and Raffaele with the 110M)

```bash 
wget https://huggingface.co/karpathy/tinyllamas/resolve/main//stories260K/stories260K.bin
```

```bash 
wget https://huggingface.co/karpathy/tinyllamas/resolve/main/stories15M.bin
```

```bash 
wget https://huggingface.co/karpathy/tinyllamas/resolve/main/stories42M.bin
```
```bash 
wget https://huggingface.co/karpathy/tinyllamas/resolve/main/stories110M.bin
```

### Compilation:

So far we just copied what was written in karpathy's repo; from now on we started compiling llama, exploring different compiler flags, then we run the executable and collected the outcomes.

To compile llama we moved from login node to sifive or milkv nodes (we compiled it in both). Usually, we did it by calling:

```bash 
srun -p mcimone-milkvs -t 00:05:00 --pty bash
```
or 

```bash 
srun -p mcimone-nodes -t 00:05:00 --pty bash
```

Once we get the node, we compiled the code as follows:


```bash 
gcc -03 -o run runc.c -lm
```
and then we run it:

```bash 
./run stories260K.bin

```

Each one used a different bin file (they are 4: stories260K.bin, stories15M.bin, stories42.bin, stories110M.bin).

we also compiled it with different flags, like -fopenmp and -ofast. I collceted everything in this table:

| parameters  | node |  flags  |  tok/s  |
| ------      | ---  | -----   | -----   |
|             |      |         |         |
|             |      |         |         |

## Performance characterization

First observation: we coudln't execute perf record command on sifive nodes.

### Perf stat



