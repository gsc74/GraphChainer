# GraphChainer

GraphChainer is an accurate aligner of long reads to a variation graph, based on co-linear chaining.

### Compiling

To compile, run these:

- Install [miniconda](https://conda.io/projects/conda/en/latest/user-guide/install/index.html)
- `git submodule update --init --recursive`
- `conda env create -f CondaEnvironment.yml`
- `conda activate GraphChainer`
- `make bin/GraphChainer`

### Running

Quickstart: `./bin/GraphChainer -t 4 -f reads.fastq -g graph.gfa -a out.gam`

Key parameters:
- `-t` Number of threads (optional, default 1).
- `-f` Input reads. Format .fasta / .fastq / .fasta.gz / .fastq.gz. You can input multiple files with `-f file1 -f file2 ...` or `-f file1 file2 ...`.
- `-g` Input graph, format .gfa / .vg. **This graph must be acyclic**, see below how to construct an acyclic graph with vg.
- `-a` Output file name. Format .gam or .json.

Parameters related to colinear chaining:
- `--speed <double>` Speed-up factor (default 1). Use >1 (<1, >0) for faster (slower), but less (more) accurate alignments. It increases (decreases) the sampling rate of fragments.
- `--colinear-split-len <int>` The length of the fragments in which the long read is split to create anchors (default 35).
- `--colinear-split-gap <int>` The distance between consecutive fragments (default 35). If `--speed` is set, then always `--colinear-split-gap = ceil(--speed * --colinear-split-len`).
- `--colinear-gap <int>` When converting an optimal chain of anchors into an alignment path, split the path if the distance in the graph between consecutive anchors is greater than this value (default 10000).

### Constructing an (acyclic) variation graph

Use [vg](https://github.com/vgteam/vg) and run:

`vg construct -t 30 -a -r {ref} -v {vcf} -R 22 -p -m 3000000`

### Datasets availability

The chromosome graphs built for the experiments of GraphChainer can be found in Zenodo at [https://doi.org/10.5281/zenodo.6587252](https://doi.org/10.5281/zenodo.6587252)

### Citation

If you use GraphChainer, please cite as:

Jun Ma, Manuel Cáceres, Leena Salmela, Veli Mäkinen, Alexandru I. Tomescu. Chaining for Accurate Alignment of Erroneous Long Reads to Acyclic Variation Graphs. Submitted, 2022

### Credits

GraphChainer is built on the excellent code base of [GraphAligner](https://github.com/maickrau/GraphAligner), which is released under [MIT License](https://github.com/maickrau/GraphAligner/blob/master/LICENSE.md). GraphAligner is described in the paper [GraphAligner: Rapid and Versatile Sequence-to-Graph Alignment](https://doi.org/10.1186/s13059-020-02157-2) by Mikko Rautiainen and Tobias Marschall.
