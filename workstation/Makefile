GITCOMMIT := $(shell git rev-parse --short=7 HEAD 2>/dev/null)
NAME=dev

all: build

build:
	docker build -t bscott/${NAME}:${GITCOMMIT} .

push:
	@echo "==> Publishing bscott/${NAME}:$(GITCOMMIT)"
	@docker tag bscott/${NAME}:$(GITCOMMIT) fatih/${NAME}:latest
	@docker push fatih/${NAME}:$(GITCOMMIT)
	@docker push fatih/${NAME}:latest
	@echo "==> Your image is now available at bscott/${NAME}:$(GITCOMMIT)"

run: kill
	docker run -it -h dev -d --net=host --rm -v /var/run/docker.sock:/var/run/docker.sock -v /home/bscott/code:/home/bscott/code -v /home/bscott/.ssh:/home/bscott/.ssh -v /home/bscott/.zsh_private:/home/bscott/.zsh_private -v /home/bscott/.zsh_history:/home/bscott/.zsh_history --cap-add=SYS_PTRACE --security-opt seccomp=unconfined --privileged --name dev bscott:dev 

ssh:
	mosh --no-init --ssh="ssh -o StrictHostKeyChecking=no -i ~/.ssh/github_rsa -p 3222" bscott@localhost -- tmux new-session -AD -s main

kill:
	docker kill dev | true


.PHONY: all build run kill
