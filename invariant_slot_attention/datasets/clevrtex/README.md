# Preprocess CLEVRTex

We first turn the individual image files into tfrecords and then further
compose those into a tensorflow_datasets (TFDS) format.

1. Download the dataset from https://www.robots.ox.ac.uk/~vgg/data/clevrtex/ and extract all files.
Note YOUR_DOWNLOAD_PATH, which should contain the following directories:

    * `clevrtex_full`
    * `clevrtex_camo`
    * `clevrtex_grassbg`
    * `clevrtex_pbg`
    * `clevrtex_vbg`
    * `clevrtex_outd`

2. `cd` your terminal into `datasets/clevrtex`.

3. Run `preprocess.py` for all splits. The following commands run either 64 or 16 preprocessor scripts in parallel. Each script creates a single TFDS shard.

    * `for i in {0..63} ; do python preprocess.py --base_read_path=YOUR_DOWNLOAD_PATH --name=full --n_dirs=50 --n_shards=64 --shard_idx=$i & done`
    * `for i in {0..15} ; do python preprocess.py --base_read_path=YOUR_DOWNLOAD_PATH --name=camo --n_dirs=20 --n_shards=16 --shard_idx=$i & done`
    * `for i in {0..15} ; do python preprocess.py --base_read_path=YOUR_DOWNLOAD_PATH --name=grassbg --n_dirs=20 --n_shards=16 --shard_idx=$i & done`
    * `for i in {0..15} ; do python preprocess.py --base_read_path=YOUR_DOWNLOAD_PATH --name=pbg --n_dirs=20 --n_shards=16 --shard_idx=$i & done`
    * `for i in {0..15} ; do python preprocess.py --base_read_path=YOUR_DOWNLOAD_PATH --name=vbg --n_dirs=20 --n_shards=16 --shard_idx=$i & done`
    * `for i in {0..15} ; do python preprocess.py --base_read_path=YOUR_DOWNLOAD_PATH --name=outd --n_dirs=10 --n_shards=16 --shard_idx=$i & done`

    A folder named `tfrecords` will be created. It will contain all directories from step 1.

4. Run `tfds build` (assuming you have tensorflow installed). A TFDS dataset will be created in `~/tensorflow_datasets`.

5. You have successfully created the CLEVRTex TFDS dataset. Our CLEVRTex configs already point to `~/tensorflow_datasets/clevr_tex`.