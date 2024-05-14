# SingleObjectGrid

A jupyter notebook containing functions to slice out tracked objects from timelapse microscopy data, using object centroids, and compile a selection together in a grid layout.
Single image input required (it can be stitched beforehand).

## Prerequisites:
Python environment with the following libraries installed:
napari[all] napari_animation numpy pandas matplotlib tqdm jupyter

### Data input:
- Timelapse imaging data. A multi-dimensional numpy array with axis order [Time, Channel, Y, X]. Typically read from an existing *.npy array (or multi-page *.tiff file).
- Tracking table associated with the imaging data. A pandas dataframe read from csv (or equivalent). Should contain at least columns: 'IDTrack', 't', 'y', 'x'. Columns x and y represent object centroids.

## Steps (sumarized):
1. Data loading
2. Tracks are filtered for minimum length as to not waste time and data volume on short track fragments (tracking errors, leaving FOV, etc).
3. All included objects are sliced from the full imaging dataset according to the reduced tracking table, at centroid coordinate +/- pixel buffer, and pasted into a new numpy array with axis order [Time, TrackID (reindexed), Channel, Y, X]. Objects not tracked for the full duration of the original data are padded with black (0-intensity) frames as to not shift time between tracks.
4. A subset of tracks is selected for inclusion into a grid of n rows by m columns, and multiple pages (sets) if n-tracks exceeds n*m. Grid and page indices are automatically assigned (sorted by TrackID), without leaving positions open.
5. Selected single-object movies are pasted into an empty numpy array of appropriate size, at their belonging grid index, and loaded into the Napari viewer.
