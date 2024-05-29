# Maestro_to_CharmmGui
This is a simple repo to guide how to process the protein-ligand structures from Maestro (Schrodinger) to be recognised by the Charmm GUI platform (https://www.charmm-gui.org/).
 Assume now there is a protein-ligand complex, which must be uploaded to Charmmgui to generate the molecular dynamic simulation inputs.

### Problem: Direct upload the protein.pdb and ligand.pdb saved from Maestro to CharmmGui sometimes failed due to structure format incompatibility. To solve this

1.export protein-ligand complex from Maestro as ``` complex.pdb ```


2. Using any text editor open ```complex.pdb```, and check the last section where the small molecule is described, replacing ```HETATM``` to ```ATOM```, also change chain name from ```A``` to ```X``` (or whatever a single capital letter that different from that of protein)

```
ATOM   5290  HZ3 LYS A 482     -10.764  27.259   2.590  1.00 40.82           H
ATOM   5291  HXT LYS A 482      -5.623  24.702  -1.858  1.00 41.02           H
TER    5292      LYS A 482
HETATM 5293  O   UNL A   1      18.711  45.411   7.137  1.00  0.00           O
HETATM 5294  C   UNL A   1      18.403  45.915   8.200  1.00  0.00           C
```
to 

```
ATOM   5290  HZ3 LYS A 482     -10.764  27.259   2.590  1.00 40.82           H
ATOM   5291  HXT LYS A 482      -5.623  24.702  -1.858  1.00 41.02           H
TER    5292      LYS A 482
ATOM   5293  O   UNL X   1      18.711  45.411   7.137  1.00  0.00           O
ATOM   5294  C   UNL X   1      18.403  45.915   8.200  1.00  0.00           C
```
Note: Maestro is weird, but not every other docking package would record ligand starting with ```HETATM```, like autodock vina may have already used ```ATOM```, in that case, you don't need to change this.

3. Split out the ligand with bash command line

grep out the ligand structure by

```
grep UNL vertical.pdb > ligand.pdb
```

4. Convert the ligand pdb format to sdf format for later use in charmmgui (yes you are supposed to already familiar with Open babel before reading this)
```
obabel -ipdb ligand.pdb -osdf -O ligand.sdf -h
```

Now, you should have three files ```complex_modified.pdb```, ```ligand.pdb```, and ```ligand.sdf``` . You can upload the first and the third to Charmmgui solution input generator, and there will be no more errors.

Upload ```complex_modified.pdb```
<img width="509" alt="image" src="https://github.com/quantaosun/maestro_to_charmmgui/assets/75652473/420e4fb1-4a56-4be3-aa61-ccf4937106ef">

tick the chain name for ligand
<img width="509" alt="image" src="https://github.com/quantaosun/maestro_to_charmmgui/assets/75652473/96f25f25-f531-45ff-9381-66ad09f58224">

upload     ```ligand.sdf```
<img width="577" alt="image" src="https://github.com/quantaosun/maestro_to_charmmgui/assets/75652473/80bd5c66-8f31-4bdf-b841-5109d80fee34">


