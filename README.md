# Meet_U-Project-Protein-Ligand-Docking
Protein Ligand Docking Meet U proejcet organized by the 4EU+ Alliance of European Universities

# Molecular Generation using Pocket2Mol

This project utilizes **[Pocket2Mol](https://github.com/pengxingang/Pocket2Mol)** for molecular generation based on a **3D protein pocket**. The first pocket of **4YPX**, identified using **fpocket**, was used as input to generate two different molecules.

## Environment Setup
Before running the generation pipeline, the environment was set up following the instructions in the original **Pocket2Mol** repository, with minor modifications for execution on the **IFB Cluster**.

### Install Dependencies
```bash
conda create -n Pocket2Mol python=3.8
conda activate Pocket2Mol
conda install pytorch==1.10.1 cudatoolkit=11.3 -c pytorch -c conda-forge
conda install pyg -c pyg
conda install -c conda-forge rdkit biopython pyyaml easydict python-lmdb
```

## Generating Molecules

### **First Molecule**
The first molecule was generated using the following configuration:

```yaml
model:
    checkpoint: ./ckpt/pretrained_Pocket2Mol.pt

sample:
  seed: 2020
  num_samples: 1
  beam_size: 50
  max_steps: 20
  threshold:
    focal_threshold: 0.3
    pos_threshold: 0.2
    element_threshold: 0.2
    hasatom_threshold: 0.5
    bond_threshold: 0.3
```
Execution command:
```bash
python sample_for_pdb.py --pdb_path ./proteins/4ypx.pdb --center " -19.50667162, 40.72684535, -9.71524278" --device cpu
```
**Generated Molecule (SMILES):**  
`O=C1N=C=NC2=NN=CC12`

---

### **Second Molecule**
A second configuration was used to generate another molecule:

```yaml
model:
    checkpoint: ./ckpt/pretrained_Pocket2Mol.pt

sample:
  seed: 2020
  num_samples: 1
  beam_size: 50
  max_steps: 20
  threshold:
    focal_threshold: 0.5
    pos_threshold: 0.25
    element_threshold: 0.3
    hasatom_threshold: 0.6
    bond_threshold: 0.4
```
Execution command:
```bash
python sample_for_pdb.py --pdb_path ./proteins/4ypx.pdb --center " -19.50667162, 40.72684535, -9.71524278" --device cpu
```
**Generated Molecule (SMILES):**  
`C1=NCOc2ncccc21`

---

## Notes
- **AutoDock Vina** was used to calculate the **binding affinity scores** for the generated molecules.
- The **first pocket of 4YPX** was selected as the input for molecular generation.
- **Additional filtering steps (toxicity analysis, thresholding) were not implemented** due to computational constraints.

For more details on **Pocket2Mol**, visit the original repository:  
🔗 [Pocket2Mol Repository](https://github.com/pengxingang/Pocket2Mol)
