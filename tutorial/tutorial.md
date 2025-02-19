# Biomechanical system for calculating brain deformations within 3D Slicer

## Patient-specific brain biomechanical model construction

This section illustrates the steps to construct a patient-specific brain
biomechanical model for craniotomy-induced brain shift and electrode
placement-induced brain shift case studies.

### Loading image data

![Loading data within 3D Slicer.](./figs/load_data.png){#fig:load_data
width="\\textwidth"}

1.  Select Slicer4 and then "Load Data" module.

2.  Select "Choose files to Add" to add files from the system. Many of
    the file formats are supported.

3.  Hit open and ok to load them in the Slicer4. The image opens in
    sagittal, axial and coronal view as shown in
    Fig [1.1](#fig:load_data){reference-type="ref"
    reference="fig:load_data"}.

### Rigid registration

In case of craniotomy-induced brain shift and tumour resection-induced
brain shift, we rigidly registered pre-operative MRI to
intra/post-operative MRI. We used the "General Registration" module
within 3D Slicer to do the rigid registration of pre-operative MRI to
intra-operative MRI.

The steps are as follows:

1.  Select the "General Registration (BRAINS)" module.

2.  Select the "Fixed Input Volume" pre-operative MRI.

3.  Select the "Moving Input Volume" intra/post-operative MRI or
    (intra-operative CT).

4.  Select the "Slicer Linear Transform" as we are using the rigid
    registration method with (6 DOF) under section "Registration
    Phases".

5.  Select the "Output Image Volume" as new volume with name of your
    choice. This volume is the intra/post-operative MRI or
    (intra-operative CT) registered to pre-operative MRI volume.

6.  Hit "Apply"

![Rigid registration results of pre-operative MRI to intra-operative CT
within 3D Slicer.](./figs/rigid_eeg.png){#fig:rigid width="\\textwidth"}

In case of electrode placement-induced brain shift, we rigidly
registered the pre-operative MRI to intra-operative CT (see
Fig. [1.2](#fig:rigid){reference-type="ref" reference="fig:rigid"}).

### Skull stripping

We performed skull stripping using FreeSurfer the results of the skull
stripped brain mask are loaded in the
Fig. [1.3](#fig:mask){reference-type="ref" reference="fig:mask"}.

![Results of skull stripping using FreeSurfer loaded into 3D
Slicer.](./figs/brain_mask.png){#fig:mask width="\\textwidth"}

### Surface construction using "Model Maker" module of 3D Slicer

![Surface model generation within 3D Slicer.
](./figs/model_maker.png){#fig:surface_model width="\\textwidth"}

After getting brain mask using FreeSurfer, create a brain surface model
(see Fig. [1.4](#fig:surface_model){reference-type="ref"
reference="fig:surface_model"}) using the ModelMaker module. The steps
are as follows:

1.  Select the "Model Maker" module from the Modules search window.

2.  Select the "Parameter set" as a new Model Maker.

3.  Select the "Input Volume" as the brain mask volume.

4.  Select the Models as new Models

5.  Select the "Model Name" for the new Model

6.  Check the check box for "Generate All Models"

7.  Select Smooth as 5% or according to how much smoothing of the model
    is required. The more the number the more smoothing. We select it to
    5%.

8.  Select the Laplacian for "Filter Type"

9.  Select Decimate to zero.

10. Uncheck "Split Normals", "Point Normals" and "Pad".

11. Let all the parameters remain the same.

12. Hit "Apply"

13. The results is a Model which is shown in the 3D window of the 3D
    Slicer.

Surface Toolbox (see
Fig. [1.7](#fig:surface_model2){reference-type="ref"
reference="fig:surface_model2"}) can be used to smooth the polygonal
surface of the model if needed.

![Surface Toolbox module within 3D
Slicer.](./figs/model_maker2.png){#fig:surface_model2
width="\\textwidth"}

### Patient-specific brain model construction

The patient-specific brain model construction begins with a surface
loaded from an external file (see
Fig. [1.6](#fig:2d_model){reference-type="ref"
reference="fig:2d_model"}). The steps are as follows:

1.  Select the "Input File" of the model as .stl file.

2.  Select the "Number of clusters". The more the number of clusters the
    more dense the constructed mesh (see
    Fig. [1.4](#fig:surface_model){reference-type="ref"
    reference="fig:surface_model"} and
    [1.7](#fig:surface_model2){reference-type="ref"
    reference="fig:surface_model2"}).

3.  Hit "Apply".

4.  The result is a redefined unified triangulation of the surface with
    the specified cluster numbers.

![Surface triangulation of patient-specific brain model (see
Fig. [1.4](#fig:surface_model){reference-type="ref"
reference="fig:surface_model"}) with 2000
clusters.](./figs/2Dmodel.png){#fig:2d_model width="\\textwidth"}

![Surface triangulation of patient-specific brain with 5000
clusters.](./figs/2Dmodel_2.png){#fig:surface_model2
width="\\textwidth"}

![A clipped, uniformly triangulated patient-specific brain surface
model.](./figs/2Dmodel_3.png){#fig:surface_model3 width="\\textwidth"}

![Patient-specific brain model filled with tetrahedral
elements.](./figs/3Dmodel.png){#fig:3d_model width="\\textwidth"}

To create a volumetric mesh with tetrahedral elements, we use
triangulated surface (see
Fig. [1.8](#fig:surface_model3){reference-type="ref"
reference="fig:surface_model3"}) to create a volumetric tetrahedral mesh
(see Fig. [1.9](#fig:3d_model){reference-type="ref"
reference="fig:3d_model"}). The steps are as follows:

1.  Select the "Patient Surface Mesh Model" as an input brain surface
    model.

2.  Select the "Mesh Algorithm" as Delaunay.

3.  Let other parameters remain default.

4.  Hit "Apply".

5.  The result is a tetrahedral mesh based on the triangulated surface
    mesh.

### Integration point generation

We used the MTLEDSimulator to generate the integration point file. The
steps are as follows:

1.  Select the model file \".inp\".

2.  Select location to save the file "Save To File" under integration
    options and write name of the file "integrationpoints.txt"

3.  Select the "Load Time" as 0.001 under Dynamic Relaxation.

4.  Leave all the parameters same.

![Fuzzy classification module along with classification results within
3D Slicer.](./figs/fuzzy.png){#fig:fuzzy width="\\textwidth"}

### Fuzzy classification for assigning material properties to integration points

The steps are as follows:

1.  Select "FuzzyClassification" module.

2.  Select the "Input Volume" as the original MRI image volume.

3.  Select the "Input Mask Volume", which is the brain mask volume
    extracted using FreeSurfer and loaded into the 3D Slicer.

4.  Select the "Input tumour Mask (Segmentation)", which is the tumour
    mask if the tumour is present.

5.  Select the "Number of Classes" (2 or 3). Two classes for ventricles
    and parenchyma and if tumour is present then three classes.

6.  Hit "Apply", the results are the ".nrrd" volumes for each class
    (ventricles, parenchyma and tumour (if present)).

### Material properties assignment

For assigning the material properties to integration points for
different tissues, we have used the following steps:

1.  Select "Input Ventricles Volume (fuzzy classified)", which is the
    fuzzy classified ventricles binary image (.nrrd).

2.  Select "Input Tumour Volume (fuzzy classified)" if present, which is
    the fuzzy classified tumour binary image (.nrrd).

3.  Select "Number of Tissue Classes", 2 or 3.

4.  Select "Material Properties"

5.  Select "Input Integration Point File", which is the integration
    point file generated by the mmls shape function using the MTLED.

6.  Select "Output Material Properties File", which is the txt file
    containing material properties assigned to each integration point.

![Material properties assignment module to integration points of fuzzy
classified tissue classes.](./figs/material.png){#fig:material
width="\\textwidth"}

### Boundary conditions, contacts and loading

We generated the skull using the brain surface layer and then combined
the skull, brain and contacts information in the .inp file to be used as
an input to MTLEDSimulator. The steps for generating the skull interface
(see Fig. [1.12](#fig:skull){reference-type="ref"
reference="fig:skull"}) are as follows:

![Skull generator module within 3D Slicer.](./figs/skull.png){#fig:skull
width="\\textwidth"}

1.  Select the "SkullGenerator" module.

2.  Select the "Input Skull", which is the brain triangulated surface
    model.

3.  Select the "Input Brain Volume", which is the tetrahedral brain
    model.

4.  Select the "Output Contacts File", which is the file containing the
    surface brain nodes in contact with the skull surface.

5.  Select the "Output Skull File", which is the file with redefined
    skull nodes and triangles.

After generating the skull file and contacts file, we create a new file
that combines skull, brain volume and the contacts using the
"SkullGenerator", "Combine skull and brain" section as follows:

1.  Select the "Input Skull File", which is the newly created txt file
    using section "Skull Generator".

2.  Select the "Input Contacts File", which is the contacts txt file
    generated using the "Skull Generator".

3.  Select the "Output brain biomechanical model (.inp) File", which is
    the final mesh file in abaqus format for input to MTLEDSimulator.

4.  Hit "Apply" to generate the file.

![MTLEDSimulator module within 3D Slicer.](./figs/mtled.png){#fig:mtled
width="\\textwidth"}

### Solution algorithm

We use the MTLEDSimulator (see
Fig. [1.13](#fig:mtled){reference-type="ref" reference="fig:mtled"}) to
calculate the displacements.

1.  Select the "MTLED Simulator" module.

2.  Select the three input files: (a) biomechanical model file, (b)
    materials file and (c) load file.

3.  Leave all other settings unchanged, hit "Apply", the results from
    MTLED can we viewed within 3D Slicer using "visualisation" module

Generate undeformed and deformed nodal coordinates to generate a
transform which can be applied to the pre-operative MRI to get the
predicted intra-operative MRI.

1.  Select the undeformed model (initial model) and generate a txt file
    that contains all the initial nodal coordinates of the initial
    undeformed model.

2.  Select the deformed model (final model) and generate a txt file that
    contains all the final nodal coordinates of the final model.

![Scattered transform module along with transformed pre-operative MRI in
axial, saggital and coronal view.](./figs/warping.png){#fig:scattered
width="\\textwidth"}

### Image warping

After you get the MTLED Simulator results, which are the model files in
each time step. We take two files the initial deformed model file and
final deformed model file. We extract all the points (node locations)
from both the models and use it to generate a transform using "Scattered
Transform" (see Fig. [1.14](#fig:scattered){reference-type="ref"
reference="fig:scattered"}) as follows:

1.  Select the "Scattered Transform" module.

2.  Select the "File with initial point positions", which is the
    position of all the nodes in an undeformed model.

3.  Select the "File with the displaced point positions", which is the
    position pf all the nodes in deformed model.

4.  Select "Output Transform" as a new B-spline transform.

5.  Leave the remaining settings and hit "Apply" to get the transform
    that can be applied to the pre-operative MRI to get the predicted
    intra-operative MRI using the "Transform" module of the 3D Slicer.

![Transform module within 3D Slicer.](./figs/warping_2.png){#fig:trans
width="\\textwidth"}

The transform module of 3D Slicer (see
Fig. [1.15](#fig:trans){reference-type="ref" reference="fig:trans"}) is
used to view the transform as follows:

1.  Select the "Transform" module.

2.  Select the "Active Transform", which is the transform generated by
    Scattered Transform.

3.  Select the image from Transformable window to be transformed and use
    the right green arrow to put under the Transformed window.

4.  Select the image in the Transformed window and harden it using the
    button at the bottom of two green arrows.

![Visualisation module within 3D Slicer along with visualisation of
craniotomy induced brain shift results.](./figs/vis.png){#fig:vis_cran
width="\\textwidth"}

### Visualisation of results

For visualisation of deformation results on the model as well as on the
volumes, we used visualisation module as follows:

1.  Select the "Visualisation" module (see
    Fig. [1.16](#fig:vis_cran){reference-type="ref"
    reference="fig:vis_cran"}).

2.  Select the "Input MRI Volume", which is the pre-operative MRI brain
    volume.

3.  Select the "Input BSpline Transform", which is the calculated
    transform using the scattered transform with inputs as undeformed
    and deformed model points.

4.  Select the "Input Model", which is the final deformed brain model as
    depicted by the MTLEDSimulator.

5.  Hit "Apply" to get the visualisation results (see
    Fig. [1.16](#fig:vis_cran){reference-type="ref"
    reference="fig:vis_cran"}).

![Visualisation module within 3D Slicer along with visualisation of
electrode placement-induced brain shift results with overlaying CT image
in foreground.](./figs/vis_3.png){#fig:vis_electrode3
width="\\textwidth"}

![Visualisation module within 3D Slicer along with visualisation of
electrode placement-induced brain shift
results.](./figs/vis_2.png){#fig:vis_electrode2 width="\\textwidth"}

For visualisation of electrode placement-induced brain shift results
(see Fig. [1.18](#fig:vis_electrode2){reference-type="ref"
reference="fig:vis_electrode2"} and
[1.17](#fig:vis_electrode3){reference-type="ref"
reference="fig:vis_electrode3"}) use the same steps as for visualisation
of craniotomy-induced brain shift deformation results. Visualization
along with CT scan and deformed model along with displacement field
mapping (see Fig. [1.17](#fig:vis_electrode3){reference-type="ref"
reference="fig:vis_electrode3"}) can be viewed by setting the foreground
image from the slicer windows.

## Patient-specific reference model construction for selecting displaced nodes on brain surface

This section illustrates the steps to construct a reference model for
selecting loaded nodes on the brain surface.

![Patient-specific craniotomy region construction
module.](./figs/cran.png){#fig:cran width="\\textwidth"}

### Case study: Craniotomy-induced brain shift

For craniotomy-induced brain shift, we need a craniotomy region to
select the nodes under the craniotomy area on the brain surface. For
this we created a module to automatically construct that craniotomy
region (see Fig. [1.19](#fig:cran){reference-type="ref"
reference="fig:cran"}). The steps are as follows:

1.  Select the "CranCreator" module.

2.  Select the "Input volume" to create a head mask for pre-operative
    MRI.

3.  Hit "Apply" to generate the preoperative head mask.

4.  Select the "Input volume" to create a head mask using
    intra-operative MRI.

5.  Hit "Apply" to generate the intraoperative head mask.

6.  Select the "Select preoperative segment", which is the head segment
    generated in step 3.

7.  Select the "Select intraoperative segment", which is the head
    segment generated in step 5.

8.  Hit "Apply" to get the craniotomy region.

![Patient-specific craniotomy region model
triangulation.](./figs/cran_2dmodel.png){#fig:cran_2dmodel
width="\\textwidth"}

Generate the surface model for this craniotomy region using "Model
Maker" module of 3D Slicer. After generating a craniotomy surface model,
generate a uniform triangulation of the craniotomy surface model using
the "SurfaceTriangulation" module (see
Fig. [1.20](#fig:cran_2dmodel){reference-type="ref"
reference="fig:cran_2dmodel"}).

![NodeSelector module within 3D Slicer along with patient-specific brain
model and craniotomy reference geometry for selecting nodes under
craniotomy on brain surface.](./figs/cran_2dmodel_2.png){#fig:node_sel
width="\\textwidth"}

![Patient-specific brain model with selected loaded nodes under
craniotomy.](./figs/load_node.png){#fig:node_sel2 width="\\textwidth"}

![Patient-specific brain model with selected nodes under craniotomy
(highlighted red region) in 3D window along with showing nodes (red
dots) in axial, sagittal and coronal view. Yellow outline around brain
in axial, sagittal and coronal view shows the boundary of the resulting
3D patient-specific brain
geometry.](./figs/load_node2.png){#fig:node_sel3 width="\\textwidth"}

![NodeSelector module outcome along with 3D brain model and 3D
craniotomy model in 3D window within 3D Slicer.
](./figs/load_node3.png){#fig:node_sel4 width="\\textwidth"}

Use the "NodeSelector" to select the brain surface nodes under
craniotomy region (see Fig. [1.21](#fig:node_sel){reference-type="ref"
reference="fig:node_sel"}). The steps are as follows:

1.  Select the "NodeSelector" module.

2.  Select the "Input Model to select Nodes", which is the craniotomy
    region model.

3.  Select the "Input Brain Model", which is the patient-specific brain
    surface triangulated model.

4.  Select the "Output Displaced Nodes", which is the txt file to save
    the loaded nodes ids.

5.  Select the "Output Cell Numbers (Triangles)", which is the txt file
    containing cell numbers.

6.  Hit "Apply" to get the selected brain nodes (loaded nodes), on which
    displacements will be applied.

The result for the "NodeSelector" are in
Fig. [1.22](#fig:node_sel2){reference-type="ref"
reference="fig:node_sel2"}.

![ElectrodesToMarkups module for selecting electrodes within 3D Slicer.
](./figs/electrode.png){#fig:electrode width="\\textwidth"}

### Case Study: Electrode placement induced brain shift

In case of electrode placement-induced brain shift, we constructed an
electrode sheet in order to select the underlying brain nodes on which
displacements are applied. The procedure begins with selecting original
electrode positions from intra-operative CT and is divided into
following steps:

1.  Select the "ElectrodesToMarkups" module (see
    Fig. [1.25](#fig:electrode){reference-type="ref"
    reference="fig:electrode"}).

2.  Select the "Input Electrode" image, which is a ".nrrd" image with
    segmented electrodes from intra-operative CT.

3.  Select the "Minimum Size (Split island)", which is the size of the
    number of voxels that defined a single electrode.

4.  Select the "Save Electrode Centroid Positions", which is the .txt
    file for saving the x,y,z coordinates for each electrode or (save
    the fiducials file .fcsv, which are the generated red dots placed at
    the identified location for each electrode).

5.  Hit "Apply"

6.  The results is the Markups/fiducials (red dots). The file of which
    can be saved separately as .fcsv file.

!["fiducialtomodeldistance" module for selecting projected electrode
locations on an undeformed brain surface with reference to original
electrode locations within 3D Slicer.
](./figs/electrode_2.png){#fig:electrode_2 width="\\textwidth"}

After you get the fiducials (red dots = identified original electrode
locations), we use these to identify the projected electrode locations
(the position of each of the electrode on an undeformed brain surface).
We use the "fiducialtomodeldistance" (see
Fig. [1.26](#fig:electrode_2){reference-type="ref"
reference="fig:electrode_2"}) for this purpose, the steps are as
follows:

1.  Select the "fiducialtomodeldistance" module.

2.  Select the "Input Markups", which are the identified original
    electrodes represented by the fiducials (red dots).

3.  Select the "Input Model", which is the patient-specific triangulated
    surface model.

4.  Hit "Apply"

5.  The result is the set of fiducials representing the projected
    electrode locations on the brain surface.

![\"Markupstosurfacemesh\" module for constructing an electrode sheet
model using projected electrode locations within 3D Slicer.
](./figs/electrode_3.png){#fig:electrode_3 width="\\textwidth"}

After getting the projected electrode locations, we construct an
electrode sheet model using the information of the locations of the
projected electrodes on the brain surface (see
Fig. [1.27](#fig:electrode_3){reference-type="ref"
reference="fig:electrode_3"}). The steps are as follows:

1.  Select "Markupstosurfacemesh" module.

2.  Select the "Input fiducials", which are the projected electrode
    locations.

3.  Select the "Output Model", which is the resulting electrode sheet
    model.

4.  Hit "Apply", to get the electrode sheet model as in
    Fig. [1.27](#fig:electrode_3){reference-type="ref"
    reference="fig:electrode_3"}.

![\"SurfaceTriangulation\" module for uniform triangulation of the
electrode sheet model within 3D Slicer.
](./figs/electrode_4.png){#fig:electrode_4 width="\\textwidth"}

After getting the electrode sheet surface model, we do the uniform
triangulation of the electrode sheet surface by using the
"SurfaceTriangulation" (see
Fig. [1.28](#fig:electrode_4){reference-type="ref"
reference="fig:electrode_4"}). The steps are as follows:

1.  Select the "SurfaceTriangulation" module.

2.  Select the "Input Model", which is the electrode sheet surface
    model.

3.  Select the "Number of clusters".

4.  Hit "Apply", to get a uniform surface triangulation of the electrode
    sheet with clusters of your choice.

![\"NodeSelector\" module for selecting nodes on undeformed brain
surface under electrode sheet model within 3D Slicer.
](./figs/electrode_5.png){#fig:electrode_5 width="\\textwidth"}

After constructing a triangulated electrode sheet model, we use it to
select the underlying brain cells and then from there we select the
corresponding brain nodes (loaded nodes). The steps are as follows:

1.  Select the "NodeSelector" module (see
    Fig. [1.29](#fig:electrode_5){reference-type="ref"
    reference="fig:electrode_5"}).

2.  Select the "Input model to select Nodes", which is the reference
    model to select the brain surface nodes i.e; electrode sheet model.

3.  Select the "Input Brain Model", the brain model on which the loaded
    nodes are selected.

4.  Select "Output Displaced Nodes", which is the .txt file of the
    selected nodes under electrode sheet model.

5.  Select "Output Cell Numbers (triangles)", which is the .txt file of
    the selected brain cells under electrode sheet model.

6.  Hit "Apply", gives you the corresponding brain nodes with fiducials
    placed at each selected brain node (see
    Fig. [1.29](#fig:electrode_5){reference-type="ref"
    reference="fig:electrode_5"}).

![\"Scattered Transform\" module for generating a B-Spline transform
using original electrode positions and projected electrode positions
within 3D Slicer. Red dots represents the transformed brain nodes.
Yellow lines represents electrode sheet surface on a deformed and
undeformed brain geometry. ](./figs/electrode_6.png){#fig:electrode_6
width="\\textwidth"}

![Displacement loading calculation based on undeformed and deformed
brain surface nodes (see
Fig. [\[fig:electrodes_8\]](#fig:electrodes_8){reference-type="ref"
reference="fig:electrodes_8"})
](./figs/electrode_7.png){#fig:electrode_7 width="\\textwidth"}

We then used the original electrode locations and projected electrode
locations to calculate a transform using scattered transform. We use
this transform and apply it on the selected brain nodes (loaded nodes)
on the brain surface to get the brain nodes on the deformed brain
surface. The steps to calculate the B-spline transform using the
scattered transform are as follows:

1.  Select the "Scattered Transform" module.

2.  Select the "File with initial point positions", which is the
    locations of the original electrode positions.

3.  Select the "File with displaced point positions", which is the
    locations of the projected electrode positions.

4.  Select "Slicer B-spline transform".

5.  Leave all other settings remain unchanged and hit "Apply", the
    result is a transform.

6.  Use the transform module to apply on the brain nodes (loaded nodes)
    to get the corresponding brain nodes locations on the deformed brain
    geometry (see Fig. [1.30](#fig:electrode_6){reference-type="ref"
    reference="fig:electrode_6"} and
    [1.31](#fig:electrode_7){reference-type="ref"
    reference="fig:electrode_7"}).

We then do load calculation using the displacement loading in
\"MeshNodesToFiducials\" module (see
Fig. [1.31](#fig:electrode_7){reference-type="ref"
reference="fig:electrode_7"} and
[1.32](#fig:electrode_8){reference-type="ref"
reference="fig:electrode_8"}).

![Undeformed brain surface nodes (blue) and deformed brain surface nodes
(red) after applying the scattered transform.
](./figs/electrode_8.png){#fig:electrode_8 width="\\textwidth"}

In case of non-rectangular electrode grid, the selection of displaced
nodes is done using the \"BrainMeshSurfaceCellsSelection\" (see
Fig. [1.33](#fig:brainCells){reference-type="ref"
reference="fig:brainCells"} and
[1.34](#fig:neighbourCells){reference-type="ref"
reference="fig:neighbourCells"})
\"BrainSurfaceNeighbouringCellsSelection\" modules.

![\"BrainMeshSurfaceCellsSelection\" module within 3D Slicer.
](./figs/brainCells.png){#fig:brainCells width="\\textwidth"}

![\"BrainSurfaceNeighbouringCellsSelection\" module within 3D Slicer.
](./figs/neighbourCells.png){#fig:neighbourCells width="\\textwidth"}

Case Study: Tumour resection-induced brain shift

The steps are as follows:

1\) Select "General Registration (BRAINS)" to register the pre-operative
to intra-operative MRI image. 2) Define the gravity direction units. 3)
Select the "TetrahedralMeshGenerator", which is used to construct the
patient-specific geometry conforming tetrahedral integration grid. 4)
Select "FuzzyClassification", which is used to classify the tissues into
different classes (brain parenchyma, ventricles, tumour) for assigning
material properties. 5) Select "MTLEDSimulator" to generate the
integration points. 6) Select "MaterialProperties" to assign material
properties to integration points using Ogden model. 7) Select
"MTLEDSimulator" to compute the reaction forces between tumour and
surrounding tissues. 8) Generate the loading file from results of step
5. 9) Reconstruct brain model with tumour cavity and generate a new
biomechanical model file for "MTLEDSimulator" 10) Repeat step 5 and 6 to
generate integration points with tumour cavity and assign material
properties. 11) Select "MTLEDSimulator" to compute brain displacement
after tumour resection. 12) Extract undeformed and deformed nodal
coordinates from the results of step 11. 13) Select "Scattered
Transform" to generate B-Spline transform for warping the pre-operative
MRI.

