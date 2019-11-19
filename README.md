# Arch Linux for MRI (MRArch)

This repo includes a guide to installing Arch Linux on a virtualbox virtual machine (archinstall.md) and scripts for installing and configuring a whole suite of neuroimaging programs on Arch Linux after install automatically.

The programs included are:
* **3D Slicer**: https://www.slicer.org/
* **SPM12**: https://www.fil.ion.ucl.ac.uk/spm/software/
* **FreeSurfer**: https://surfer.nmr.mgh.harvard.edu/
* **FSL**: https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/
* **AFNI**: https://afni.nimh.nih.gov/
* **ANTs**: https://github.com/ANTsX/ANTs
* **BART**: https://mrirecon.github.io/bart/bin
* **DCM2BIDS**: https://github.com/cbedetti/Dcm2Bids

and multiple **nipy** (https://nipy.org/) python library initiatives, notably:
* **nibabel**: https://nipy.org/nibabel/
* **nipype**: https://nipype.readthedocs.io/en/latest/bin

Nipype is a particularly useful and important part of this installation as it allows one to create custom pipelines which seamlessly mix and match functions from the neuroimaging suites. The install guide includes additional instructions for installing MATLAB prior to SPM12 and nipype as this cannot be automated due to the restrictive MATLAB download and licensing.

The GNOME desktop environment (https://www.gnome.org/) is included to provide a graphical interface but can be removed or replaced for personal preference. A list of DEs which available for Arch Linux is provided here: https://wiki.archlinux.org/index.php/Desktop_environment .

Note: none the programs installed are provided by me, and each has its own respective liscnense and should be cited appropriately when used. Most are installed via the Arch User Repository AUR (https://aur.archlinux.org/) which simply downloads the programs from the source websites and does not distribute the software itself.

