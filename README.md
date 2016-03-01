# Alpine-serverspec

an alpine based serverspec Docker image to run tests on your images/dockerfile.

*note: this image can be used to run tests on your server/vm/cloud too*


## Introduction
Serverspec is a ruby TDD (Test Driven Development) tool based on rspec.
it's been used for years to test infractructure compliancy.

Using Serverspec you can test :

- system and/or middleware configurations
- running services and port listening
- strings in files
- account creation
- package installation
.....

for more information about Serverspec please look at the [documention page](http://serverspec.org/) and the [tutorial](http://serverspec.org/tutorial.html)

## Documentation

### Existing spec files
if your used to it, then this easiest way to use this container is to mount directory where your spec files are stored in the alpine-serverspec container:

`docker run --rm -v [PATH TO YOUR SPEC FILES]:/project rzarouali/alpine-serverspec rake`

### first step
If it's your first time with serverspec it's ok too :)
```
docker run --rm -it -v [YOUR DOCKER PROJECT]:/project rzarouali/alpine-serverspec:develop sh
mkdir spec
cat <<\EOF>> spec/Dockerfile_spec.rb
require "serverspec"
require "docker"

describe "Dockerfile" do
  before(:all) do
    image = Docker::Image.build_from_dir('.')

    set :os, family: :debian
    set :backend, :docker
    set :docker_image, image.id
  end

  it "installs the right version of Debian" do
    expect(os_version).to include("Jessie")
  end

  def os_version
    command("lsb_release -a").stdout
  end
end
EOF
```
this will give you, a quick template to test your base image (Debian Jessie here).

you'll find more informations about types and how to test with serverspec in the [types documentation](http://serverspec.org/resource_types.html)
