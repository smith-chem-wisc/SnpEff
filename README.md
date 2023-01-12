# SnpEff for Spritz

This fork of SnpEff has been adapted for proteogenomics to be used in [Spritz](https://smith-chem-wisc.github.io/Spritz/).

## SnpEff Documentation

> http://snpeff.sourceforge.net/
> 
> Enjoy! 
> 
> Pablo Cingolani

## Usage example

1. Please download the [latest release](https://github.com/smith-chem-wisc/SnpEff/releases/latest)
2. Once unzipped, this folder should have the file `snpEff.jar` binary in its base directory.
3. You can then use [commands such as those used in Spritz](https://github.com/smith-chem-wisc/Spritz/blob/f27b14a84a75ab3d20d12e3645256600f3a9ef9c/Spritz/workflow/rules/variants.smk#L203) to generate a proteogenomic database for variants, e.g., `java -Xmx16000M -jar snpEff.jar -v -nostats -fastaProt output.protfa -xmlProt output.protxml reference.gff3 input.vcf > output.ann 2> log.txt`

## Building information (as of June 2020)

To build SnpEff for Spritz:
1. Activate spritz conda environment
2. `conda install maven`
3. Execute the following commands:

```
cd SnpEff
mvn install:install-file -Dfile=lib/antlr-4.5.1-complete.jar -DgroupId=org.antlr -DartifactId=antlr -Dversion=4.5.1 -Dpackaging=jar
mvn install:install-file -Dfile=lib/biojava3-core-3.0.7.jar -DgroupId=org.biojava -DartifactId=biojava3-core -Dversion=3.0.7 -Dpackaging=jar
mvn install:install-file -Dfile=lib/biojava3-structure-3.0.7.jar -DgroupId=org.biojava -DartifactId=biojava3-structure -Dversion=3.0.7 -Dpackaging=jar
export VERSION=4.3
export VERSION_UND=`echo $VERSION | tr '.' '_'`
mvn clean compile assembly:assembly
mvn install:install-file -Dfile=target/SnpEff-$VERSION.jar -DgroupId=org.snpeff -DartifactId=SnpEff -Dversion=$VERSION -Dpackaging=jar -DgeneratePom=true --quiet
cp target/SnpEff-$VERSION-jar-with-dependencies.jar snpEff.jar
cd ..
```

4. To create new zip-file release, include everything except the `data` folder.
