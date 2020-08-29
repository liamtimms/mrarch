# MRI software on Arch Linux (MRArch)

This repo includes a guide to installing Arch Linux on a virtualbox virtual machine (archinstall.md) and scripts for installing  most major neuroimaging programs on Arch Linux after install automatically. It's designed for use in a VM but can also easily be applied to bare metal installs for better performance (you will need to edit the `MRArch` bashscript to change from the vmware display driver).

The programs included in the full setup are:
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
* **nilearn**:
* **nipype**: https://nipype.readthedocs.io/en/latest/bin

Nipype is a particularly useful and important part of this installation as it allows one to create custom pipelines which seamlessly mix and match functions from the neuroimaging suites. The install guide includes additional instructions for installing MATLAB prior to SPM12 and nipype as this cannot be automated due to the restrictive MATLAB download and licensing.

The GNOME desktop environment (https://www.gnome.org/) is included to provide a graphical interface but can be removed or replaced for personal preference by editing the end of the first MRArch bash-script.

The list of desktop environments which are available for Arch Linux is here: https://wiki.archlinux.org/index.php/Desktop_environment . (Cinnamon provides a more Windows-like experience)

**Important Note:** none of the programs installed by this script are actually provided by me. Most are installed via the Arch User Repository AUR (https://aur.archlinux.org/) which provides links to each program from the source website and does not distribute any software itself. *Each of these programs carries its own license and must be cited appropriately when used.* Please refer to the documentation for each tool for more information.

