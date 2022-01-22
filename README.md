# Zadanie 2 PFCO

#### [Link do repozytorium GitHub](https://github.com/lopinni/Zad2PFCO)
#### [Link do repozytorium DockerHub](https://hub.docker.com/repository/docker/lopinni/zad2pfco)
#### [Link do cache z części 4](https://hub.docker.com/repository/docker/lopinni/app)

Zadanie 2 zostało wykonane na podstawie projektu udostępnionego w laboratorium 11.

### Część 1

Część 1 wykorzystuje dockerfile.v1 zawarty już w projekcie z lab 11. Plik yml do budowania znajduje się [tutaj](https://github.com/lopinni/Zad2PFCO/blob/main/.github/workflows/docker-image.yml).

### Część 2

Część 2 wykorzystuje plik yml z części 1 ze zmianami:
- dodano część z QEMU
```
- name: Set up QEMU
  uses: docker/setup-qemu-action@master
  with:
  platforms: all
```
- dodano platformy do sekcji "Build and push"
```
- name: Build and push
    id: docker_build
    uses: docker/build-push-action@v2
    with:
        context: ./
        file: ./dockerfile.v1
        platforms: linux/amd64, linux/arm64/v8, linux/arm/v7 
        push: true
        tags: |
        lopinni/zad2pfco:cz2
```
Plik yml do części 2 znajduje się [tutaj](https://github.com/lopinni/Zad2PFCO/blob/main/.github/workflows/docker-image2.yml)

### Część 3

Część 3 wykorzystuje plik yml z części 1 z tym, że używany jest inny dockerfile, który także był już w projekcie:
```
file: ./Dockerfile.zad2
```
Tak więc, odpowiadając na pytanie, wieloetapowe budowanie obrazów jest możliwe w połączeniu z GithubActions. Plik yml do części 3 znajduje się [tutaj](https://github.com/lopinni/Zad2PFCO/blob/main/.github/workflows/docker-image3.yml)

### Część 4

Wykorzystywanie cache także jest możliwe z GithubActions. Wystarczy dodać repozytorium DockerHub z cache:
```
- name: Build and push
    id: docker_build
    uses: docker/build-push-action@v2
    with:
        context: ./
        file: ./dockerfile.v1
        push: true
        tags: lopinni/zad2pfco:cz4
        cache-from: type=registry,ref=lopinni/app:buildcache
        cache-to: type=registry,ref=lopinni/app:buildcache,mode=max
```
Repo z cache znajduje się [tutaj](https://hub.docker.com/repository/docker/lopinni/app), a plik yml [tutaj](https://github.com/lopinni/Zad2PFCO/blob/main/.github/workflows/docker-image4.yml)
