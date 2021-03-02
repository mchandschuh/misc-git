# misc-git

Provides random scripts mainly focused on git productivity but slowly growing to include other random helpful things.

## Installation

Simply clone this repo and add the root directory to your PATH environment variable (recommend system PATH). These scripts will then be available.

### git-open

Opens a file using shell execute. If no arguments are provided, you will be prompted for a default file. I set this up to be my visual studio solution file. If the solution file is `./src/solution.sln` then when prompted you'll need to enter `src\\solution.sln`.

The default file is saved via `git config open.default '<value>'` and can be reset via `git config --unset open.default`

First time usage:
``` bash
Admin@DESKTOP-NC7P4UR MINGW64 /c/src/1konto/heman (master)
$ git open

  No default file is specified.
  Please specify a default file:

    src\\heman.sln
READ: src\heman.sln
opening file src\heman.sln
```

Once configured, simply call:

``` bash
git open
```

## git-pack-local

Runs `dotnet pack` and saves the packages to a local directory. The defaults on this one are very much geared towards my system with a default output directory of: `/c/src/1konto/packages`. The default solution file is found via `git config open.default`. The last argument is added to the `dotnet pack` call in case you want to do something like `--include-source`. You can also specify all the arguments. The following would override the default `-c Debug` and build in release mode as well as include source files in the packages which is great for debugging locally.

``` bash
git pack-local 18.0.0 ./src/heman.sln /c/src/1konto/packages/heman "--include-source -c Release"`
```

If you're good with all the defaults:

``` bash
git pack-local 18.0.0
```

## git-nuget-cache-clean

Searches a directory for files/folders matching a given string and deletes it. I use this when iterating on a new core library version that's being used in another project. The directory is optional and defaults to the standard nuget package directory: `~./.nuget/packages`.

For example, if I'm working on a new version `18.0.0` of a library, I would run the following to delete it in preparation of creating new packages:

``` bash
git nuget-cache-clean 18.0.0
```

### kubernetes-dashboard

 Added recommended.yaml

Dashboard file to be installed via:

``` bash
kubectl apply -f ./recommended.yaml
```

and then you can verify that it's actually running via:

``` bash
kubectl get -f ./recommended.yaml.txt
```

To access the dashboard, run the following from powershell:

``` powershell
((kubectl -n kube-system describe secret default | Select-String "token:") -split " +")[1]
```

Copy the generated token.
Run:

``` bash
kubectl proxy
```

Open the following link in your browser:

> http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

Select token and paste the token copied from the powershell command and sign in.

You can find these directions at:

> https://dev.to/devcrafter91/how-to-install-kubernetes-on-windows-10-55b6