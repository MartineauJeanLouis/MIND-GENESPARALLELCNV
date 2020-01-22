
### Docker with QuantiSNP
###### by Kristian Agbogba @ kristian.agbogba.1@etsmtl.net
___
**1) Installing Docker and loading the Docker image**

A docker image with QuantiSNP is available in the following [LINK](https://www.dropbox.com/s/i15rhnmjbdy6opc/ubuntu16-cnvcalling.tar?dl=0).

One should download the image and then load it in to their docker environment. If docker is not installed, follow the instructions in this [LINK](https://docs.docker.com/install/linux/docker-ce/ubuntu/).

After docker was installed, run the following command in order to load the image:
```
docker load -i ubuntu16-cnvcalling.tar 
```
**Note:** *For the following command to work, the image "ubuntu16-cnvcalling.tar" must be in the current directory of the terminal. If not specify the absolute path to the directory containing the image.*

**2) Testing the image**

To test if the image was properly installed run the following command:
```
docker images -a
```
You should see the following image in the output:
```
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
ubuntu-cnvcalling      Dockerfile          xxxxxxxxxxxx        x months ago        2.85GB
```

To test the terminal of the image, run the following command:
```
docker run --rm -it ubuntu-cnvcalling:Dockerfile bash
```

Output:

```
root@xxxxxxxxxxxx:/home/SOFTWARE/cnvcalling# 
```

This command should redirect your terminal to the image based terminal where QuantiSNP is installed and ready to run. Note that current directory should be */home/SOFTWARE/cnvcalling*. 

**3) Opening bash while sharing current directory with docker container**

In order to open bash and also share the current directory, run the following command:
```
docker run -v $PWD:/home/SOFTWARE/cnvcalling/host_current_dir --rm -it ubuntu-cnvcalling:Dockerfile bash
```
Now try running the following command in the docker image:

```
cd  host_current_dir/
```

Output:

```
root@xxxxxxxxxxxx:/home/SOFTWARE/cnvcalling/host_current_dir/# 
```

If one runs:
```
ls -l
```
the current directory should contain the content of the host's current directory (before opening the image terminal).

**4) QuantiSNP usage** 

For QuantiSNP usage: [https://sites.google.com/site/quantisnp/howto](https://sites.google.com/site/quantisnp/howto)

**Happy computing!**
