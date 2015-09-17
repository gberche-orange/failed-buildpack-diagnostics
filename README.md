# failed-buildpack-diagnostics 

An adaptation of [heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi) to ease diagnostics of failed buildpack execution in cloudfoundry.



## Usage
    # Initialize dummy content if needed
    $ rm -rf single_file_dir/; mkdir single_file_dir; cd single_file_dir; echo "hello.txt" > index.txt;
    
    # reference the list of faulty buildpack(s) you'd like to get more traces from
    $ echo "https://github.com/gberche-orange/sample_invalid_buildpack" > .buildpacks;
      
    $ cf push custom-buildpack  -b https://github.com/gberche-orange/failed-buildpack-diagnostics.git
    
    Updating app custom-buildpack in org orange / space gberche as gberche...
    OK
    
    Uploading custom-buildpack...
    Uploading app files from: C:\code\workspaceElPaasov14\container_info\single_file_dir
    Uploading 317, 2 files
    Done uploading
    OK
    
    Starting app custom-buildpack in org orange / space gberche as gberche...
    -----> Downloaded app package (4.0K)
    -----> Downloaded app buildpack cache (4.0K)
    Cloning into '/tmp/buildpacks/heroku-buildpack-multi'...
    =====> Downloading Buildpack: https://github.com/gberche-orange/sample_invalid_buildpack
    =====> cloning repo with git clone https://github.com/gberche-orange/sample_invalid_buildpack /tmp/buildpackd4FOk
    Cloning into '/tmp/buildpackd4FOk'...
    =====> executing /tmp/buildpackd4FOk/bin/detect /tmp/staged/app
           /tmp/buildpackd4FOk/bin/detect content is:
    #!/usr/bin/env bash
    echo "A dummy framework name"
    =====> Detected Framework: A dummy framework name
    Compilation
    =====> executing /tmp/buildpackd4FOk/bin/release /tmp/staged/app
           /tmp/buildpackd4FOk/bin/release content is:
    #!/usr/bin/env bash
    echo "an invalid Release spec: should be YAML formatted"
           release command result (expecting YAML hash) is:
    an invalid Release spec: should be YAML formatted
    Using release configuration from last framework (A dummy framework name).
    -----> Uploading droplet (4.0K)
    
    0 of 1 instances running, 1 down
    0 of 1 instances running, 1 down
    0 of 1 instances running, 1 down


## Sample normal faulty execution lacking diagnostics in v214


    $ mkdir single_file_dir; cd single_file_dir; echo "hello.txt" > index.txt; 
    $ cf push custom-buildpack  -b https://github.com/gberche-orange/sample_invalid_buildpack
    Updating app custom-buildpack in org orange / space gberche as gberche...
    OK
    
    Uploading custom-buildpack...
    Uploading app files from: C:\code\workspaceElPaasov14\container_info\single_file_dir
    Uploading 142, 1 files
    Done uploading
    OK
    
    Stopping app custom-buildpack in org orange / space gberche as gberche...
    OK
    
    Starting app custom-buildpack in org orange / space gberche as gberche...
    -----> Downloaded app package (4.0K)
    -----> Downloaded app buildpack cache (4.0K)
    Cloning into '/tmp/buildpacks/sample_invalid_buildpack'...
    Staging failed: Buildpack compilation step failed
    
    FAILED
    BuildpackCompileFailed
    
    TIP: use 'cf.exe logs custom-buildpack --recent' for more information

## License

MIT
