Use [PwnableHarness](https://github.com/C0deH4cker/PwnableHarness), For challenges where you already have a binary/only have a binary (thanks for the asm challenge, Zopazz), run 

`docker build --build-arg CHALLENGE_NAME=(name of the challenge) --build-arg CHALLENGE_PATH=(put path to the binary here) --build-arg PORT=(its just a port mate) -t opendoors .`

PwnableHarness can also build c files+create the docker, just clone the project, and run `make start-docker`