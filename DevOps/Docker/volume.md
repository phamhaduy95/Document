bind mounts attach a user-specified location  on the host filesystem to a specific point in a container file tree.

set exclusive permission for data volume so that it can not be accessible by host
Start up a data container and use the --volumes-from flag when running other  
containers

to remove unused container which is stop, type command docker container prune 
#### in-memory storage
mount volume to memory-based filesystem such as `tmpfs` to store sensitive data such as password, configuration and secrets.
you can set the destination and it size limit with `tmpfs`


Semantically, a volume is a tool for segmenting and sharing data that has a scope or life  
cycle that’s independent of a single container

Mounts the   Docker socket so  you can interact  with the Docker  daemon from  within the  
container

Integrate caching into your CI/CD pipeline

For automated builds, you should use the caching features of your CI platform (e.g., GitHub Actions, GitLab CI, CircleCI). This ensures that dependencies are cached between build runs, even if the runner environment is fresh each time. 

- **How it works (example with GitHub Actions):**
    1. In your workflow file, use the `actions/cache` action.
    2. Cache the npm cache directory (`~/.npm`) using a unique key, often based on a hash of your `package-lock.json` file. This ensures the cache is only restored if the dependencies haven't changed.
    3. Subsequent runs will use the cached files if the `package-lock.json` file is unchanged, dramatically speeding up the `npm install` or `npm ci` step.
- **Best for:** Automating deployments and builds in a CI/CD environment.
- **Recommendation:** Use `npm ci` instead of `npm install` in CI environments. `npm ci` is designed for clean installations, using the `package-lock.json` file to install the exact same versions of dependencies, making it faster and more reliable for automated builds.

Simulating troublesome networks

u have bigger problems, like a lack of memory!  
On the subject of simulating a network with many machines, there’s a particular  
kind of network failure that becomes interesting at this scale—a network partition.  
This is when a group of networked machines splits into two or more parts, such that  
all machines in the same part can talk to each other, but different parts can’t communicate. Research indicates that this happens more than you might think, particularly  
on consumer-grade clouds!


Bind mounts are mount points used to remount parts of a filesystem tree onto other  
locations. When working with containers, bind mounts attach a user-specified location  
on the host filesystem to a specific point in a container file tree

This example touches on an important feature of volumes. When you mount a volume on a container filesystem, it replaces the content that the image provides at that  
location. By default, the nginx:latest image provides some default configuration at  
/etc/nginx/conf.d/default.conf, but when you created the bind mount with a destination at that path, the content provided by the image was overridden by the content  
on the host. This behavior is the basis for the polymorphic container pattern discussed later in the chapter


volume

By default, Docker creates volumes by using  
the local volume plugin. The default behavior will create a directory to store the contents of a volume somewhere in a part of the host filesystem under control of the  
Docker engine.

Anonymous volumes can be cleaned up in two ways. First, anonymous volumes are  
automatically deleted when the container they were created for are automatically  
cleaned up. This happens when containers are deleted via the docker run --rm or  
docker rm -v flags. Second, they can be manually deleted by issuing a docker volume  
remove command:


Nmap is a powerful network inspection tool that can be used to scan network address  
ranges for running machines, fingerprint those machines, and determine what services  
they are running