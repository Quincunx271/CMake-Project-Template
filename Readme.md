# CMake Project Template

My project template for CMake based projects

# Usage

At this point, the best usage seems to be:

```
$ wget https://github.com/Quincunx271/CMake-Project-Template/archive/master.zip
$ unzip master.zip
$ mv CMake-Project-Template-master/{,.}* .
$ rmdir CMake-Project-Template-master
```

For a version that is non-header only:


```
$ wget https://github.com/Quincunx271/CMake-Project-Template/archive/library.zip
$ unzip library.zip
$ mv CMake-Project-Template-library/{,.}* .
$ rmdir CMake-Project-Template-library
```

After unpacking, run the following commands to replace the `<PROJECT>` bits with your project name,
pretending your project is named "Proj":

```
$ find . -type f \( ! -path './.git/*' \) -exec sed -Ei 's/<Project>/Proj/g' \{\} \;
$ find . -type f \( ! -path './.git/*' \) -exec sed -Ei 's/<project>/proj/g' \{\} \;
$ find . -type f \( ! -path './.git/*' \) -exec sed -Ei 's/<PROJECT>/PROJ/g' \{\} \;
$ find . \( ! -path './.git/*' \) -name '<Project>*' -exec bash -c "(echo '{}' | sed -E 'p;s/<Project>/Chef/g' | xargs -L2 mv)" \;
```
