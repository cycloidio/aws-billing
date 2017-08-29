# Aws-billing: AWS billing importer [![Build Status](https://travis-ci.org/cycloidio/aws-billing.svg?branch=master)](https://travis-ci.org/cycloidio/aws-billing) [![Coverage Status](https://coveralls.io/repos/github/cycloidio/aws-billing/badge.svg)](https://coveralls.io/github/cycloidio/aws-billing)

## What is AWS-Billing?

AWS-Billing is a project aiming to make the download & import of AWS billing data into a backend.

Currently the project provides both interfaces as well as their implementations, the idea behind however is to make it more like a library allowing everyone to import their data into the backend that they want. The implemention would then become an 'extra' or example of the overall project.

The main components are:
* Checker - check if the data needs to be imported
* Downloader - download & unzip data from S3
* Injector - responsible for injecting the data into the backend
* Loader - read the data and uses the injector in order to load it progressively

A last component is present:
* Manager - a wrapper of all the previous component

Please see notes regarding the Manager.

Any contributions are welcome as usually!

## Getting started

### Import the library/code
To get started, you can download/include the library to your code:
```go
import 	"github.com/cycloidio/aws-billing"
```

### Create a manager

As mentioned previously the Manager isn't necessary, you could skip it and create individual components.
```go
dynamoDBAccount := &billing.AwsConfig{
	AccessKey: "xxxxxxxxxxxxxxxxxx,
	SecretKey: "xxxxxxxxxxxxxxxxxx,
	Region:    "xxxxxxxxx",
}
s3Account := &billing.AwsConfig{
	AccessKey: "xxxxxxxxxxxxxxxxxx,
	SecretKey: "xxxxxxxxxxxxxxxxxx,
	Region:    "xxxxxxxxx",
}

s3Bucket := "YOUR-BUCKET-NAME"

m, err := billing.NewManager(dynamoDBAccount, s3Account)
if err != nil {
	fmt.Println(err)
	return err
}

err = m.ImportFromS3("2017-08", s3Bucket)
if err != nil {
	fmt.Println(err)
	return err
}
```

### Create sets of components

Instead of using the manager, the components could also be instanciated individually and used under your own logic.

### Makefile

In order to use the project a handful set of rules have been provided:

* cov: display the code coverage in CLI
* htmlcov: display the code coverage in a browser
* test: check that the project's test are passing
* fmtcheck: check that all files passe goimports format checks
* vetcheck: check that all files passe vet checks

## Notes

### Billing data type
Currently only the detailed-billing data with tags and resources is being imported.

### Limitations
There is currently a too strong dependency around the backend from the `Checker` component.
The goal is to move the dependency towards the injector, in order to make it easier to use/switch backend if need be.

### Manager
The manager isn't required in the library itself, it simply provides an implementation/example of how the components interract and make it easy to use. 

## License

Please see [LICENSE](LICENSE).

