---
title: "Images from scratch"
linkTitle: "ScratchImages"
weight: 4
description: >
  Use Luet to compose images from scratch
---



```bash
cat <<EOF > $PWD/luet.yaml  
system-repositories: 
                   - name: "main"
                     type: "http"
                     uri: "https://your-repo.org/path"
EOF

docker rm luet-runtime-test || true
docker run --name luet-runtime-test \
       -ti -v /tmp:/tmp \
       -v $PWD/luet.yaml:/etc/luet/.luet.yaml:ro \
       quay.io/luet/base install <package>
 
docker commit luet-runtime-test luet-runtime-test-image

# Try your new image!

docker run -ti --entrypoint /bin/sh --rm luet-runtime-test-image
```