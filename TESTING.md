## Testing

The following components of RepoLib should be tested with each revision:

### CLI - apt-manage command

The `apt-manage` command should be tested every single revision to ensure that 
it is working correctly. This will also generally test that changes to the 
underlying library have not affected anything. 

#### Adding repositories

Adding repositories should be tested. 

1. Test Adding PPAs

```
sudo apt-manage -b add ppa:system76/proposed
```
Verify that the information output and the resulting deb lines (at the end of
the output) appear correct for the given PPA.

```
sudo apt-manage -b add -s ppa:system76/proposed
```
Verify that the `deb-src` repository is uncommented in the command output.

```
sudo apt-manage -b add -d ppa:system76/proposed
```
Verify that both `deb-src` and `deb` repositories are commented out in the 
command output.

After each of the preceeding tests, ensure that no changes are made to the 
system in `/etc/apt/sources.list.d`

```
sudo apt-manage add -e ppa:system76/proposed
```
Verify that the command asks for verification before completing and displays
information about the PPA (similar to `add-apt-repository`). Verify that the 
command fetches the signing key and adds it to the system. Verify that the 
the correct `.list` file is added to `/etc/apt/sources.list.d`


2. Test adding deb repositories 

```
sudo apt-manage add deb http://example.com/ubuntu focal main
```
Verify that the added repository matches the given input, and that there is a
commented-out `deb-src` repository with it. 

3. Test adding URLs

```
sudo apt-manage add http://example.com/ubuntu
```
Verify that the repository is correctly expanded to include the `deb` at the 
beginning, and the correct `{RELEASE} main` suites and components, where 
{RELEASE} matches the current version codename. Verify that a matching `deb-src`
entry is added as well in the command output. 

#### Test Listing Details

1. Test that all repos are listed

```
apt-manage list
```
Verify that all 3rd-party repositories added to `/etc/apt/sources.list.d` are 
printed in the command output.

2. Test that details for all repositories are listed

```
apt-manage list -v
```

Verify that the specific configuration for each repository listed in step 1 is
presented in the output.

3. Test that details for a specific repository are listed

```
apt-manage list example-com-ubuntu
```

Verify that the detailed configuration for only the specified repository is 
listed in the output.

4. Test that contents of `/etc/apt/sources.list` are output

```
apt-manage list -l
cat /etc/apt/sources.list
```

Verify that in addition to the main sources, the repositories in the system-wide
`/etc/apt/sources.list` are presented in the output, and that they match the 
entries in the file.