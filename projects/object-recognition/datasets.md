## Datasets

Face databases are widely available on the Internet. Below are some sites to explore:
- http://web.mit.edu/emeyers/www/face_databases.html
- http://face-rec.org/databases/

Feel free to experiment with datasets. Our testing script has only been used with the ORL database, so it might not work with others, but it should be little trouble to adjust the script and/or the dataset to work with each other.

The object recognition system supports [PGM and PPM images](https://en.wikipedia.org/wiki/Netpbm_format), so if you are testing a new dataset which stores images in a different format, you will need to convert them first. We have a script to batch convert directories of images with ImageMagick:
```
./scripts/convert-images.sh [src-folder] [dst-folder] jpeg ppm
```

### FERET

_Note: the FERET dataset cannot be downloaded directly from the Internet. We have the archive saved in our lab, so if you want to work with FERET, you can retrieve the archive that way._

```
./scripts/get_feret.py
./scripts/create-sets.py -d feret -t [train-percentage] -r [test-percentage]
```

### MNIST

```
./scripts/get_mnist.py
./scripts/create-sets.py -d mnist -t [train-percentage] -r [test-percentage]
```

### ORL

```
./scripts/get_orl.sh
./scripts/create-sets.py -d orl -t [train-percentage] -r [test-percentage]
```
