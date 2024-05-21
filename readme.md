
# Montecimone: Llama2 performance characterization 

## Llama2 compilation on MonteCimone 

### Set up

Firstly, to set up everything we need for the compilation, we all cloned Karpathy's repository as follows:

```bash 
git clone https://github.com/karpathy/llama2.c.git
```
and entered to llama2 folder
```bash 
cd llama2.c

```

From now on, each of us worked on a different model: I decided to work on the one with 260K parameters, Lorenza with 15M, Luca with 42M and Raffaele with 110M.

In my case, i got the 260K parameters model as follows:

```bash 
wget https://huggingface.co/karpathy/tinyllamas/resolve/main/stories260K/stories260K.bin
```

For the 15M model:

```bash 
wget https://huggingface.co/karpathy/tinyllamas/resolve/main/stories15M.bin
```
and so on for the others.


### Compilation:

This first part was straightforward, we just followed what was written in Karpathy's repo. To compile llama we moved from the login node to sifive or milkv nodes. Usually, this is done with the following command:

```bash 
srun -p mcimone-milkvs -t 00:05:00 --pty bash
```
For Sifive nodes instead:

```bash 
srun -p mcimone-nodes -t 00:05:00 --pty bash
```

For what concerns the compilation, we compiled llama with different compiler flags:

- -O3 flag: 

```bash 
gcc -O3 -o run run.c -lm
```

- -fopenmp for enabling parallelism:

```bash 
gcc -O3 -fopenmp -o run run.c -lm
```

- -Ofast for ruther optimization:

```bash 
gcc -O3 -Ofast -fopenmp -o run run.c -lm
```


Finally, we run the executable and collected the outocomes:


```bash 
./run stories260K.bin

```

To sum up, we collected everything in tables:

SiFives:

| parameters |   flags                |  tok/s          |
| ------     |    -----               | -----           |
|      260K  |  -O3                   | 301.775148      |
|    260k    |     -O3 -fopenmp       |      678.191489 |
|    260k    |   -O3 -fopenmp -Ofast  |    718.309859   | 
|   15M      |   -O3                  |  4.786056       |
|   15M      |   -O3 -fopenmp         |  16.980146      | 
|   15M      |  -O3 -fopenmp -Ofast   |  17.170330      | 
|   42M      |  -O3                   | 1.768644        | 
|   42M      |   -O3 -fopenmp         |      6.462453   |
|   42M      |   -Ofast               |   6.486090      | 

 
Milkvs:

| parameters |   flags                |  tok/s          |
| ------     |    -----               | -----           |
|   260K     |   -O3                  |    876.288660   |
|   260k     |   -O3 -fopenmp         |    508.547009   |
|   260k     |   -O3 -fopenmp -Ofast  |  580.865604     | 
|   15M      |   -O3                  |  35.754347      |
|   15M      |   -O3 -fopenmp         |  158.385093     | 
|   15M      |   -O3 -fopenmp -Ofast  |  161.616162     | 
|   42M      |   -O3                  |    14.027087    | 
|   42M      |   -O3 -fopenmp         |   39.293380     |
|   42M      |   -Ofast               |   38.860104     | 


## Performance characterization

### Perf stat
First perf command we tried is **perf stat**.

Considering Sifives, we run perf stat to all models and collected the outcomes:




### Perf record
Note: we coudln't execute perf record command on sifive nodes.

