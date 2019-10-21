# workshop

## setup

```bash
brew install hugo
```

## build

```bash
git submodule init ; git submodule update

npm install

hugo -v
hugo -v -d docs
```

## start

```bash
hugo server -w -v --enableGitInfo --bind=0.0.0.0 --port 8080 --navigateToChanged
```
