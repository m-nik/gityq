# gityq
This image simply can use for deploy stage in CI/CD pipelines
```
docker pull mojprogrammer/gityq:v2.49.1
```

### Sample sage
```yaml
dev-deploy:
  stage: dev-deploy
  image:
    name: mojprogrammer/gityq:v2.49.1
    entrypoint: [ "/bin/sh", "-c" ]
  before_script:
    - git config --global user.email "ci-bot@mydev.io"
    - git config --global user.name "CI-Bot"
  script:
    - git clone https://${GIT_CREDENTIALS}@${GIT_REPO_ADDRESS} && cd ${REPO_NAME}
    - yq e ".${PROJECT}.image.tag=\"${TAG}\"" -i dev/${PROJECT}/values.yaml
    - git add dev/${PROJECT}/values.yaml
    - (git status | grep "nothing to commit, working tree clean") && echo "nothing to commit" && exit 0
    - git commit -m "Update ${PROJECT} image tag to ${TAG}"
    - git push origin main
```
