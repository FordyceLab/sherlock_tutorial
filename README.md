# Fordyce lab Sherlock submission tutorial

This tutorial will walk you through submitting your first job to Stanford's Sherlock cluster. We'll be aligning reads from a sample MiSeq run to the E. coli genome. This is not intended to be a complete Sherlock tutorial; it's just meant to get you started. For a more in-depth treatment check out the [tutorial from Possu Huang's lab](https://github.com/ProteinDesignLab/protein-design-tutorials/blob/master/intro_to_sherlock.md). 

## Logging in to Sherlock

To login to Sherlock you'll need your SUNet ID, password, and your 2-factor authentication device. For the purposes of this tutorial, fill in the relevant information whenever you see angle brackets. For instance, where you see `<SUNet ID>` fill in your SUNet ID, not the literal string `<SUNet ID>`. Alright, now that everyone knows the directions, let's get after it, shall we?

### Accessing a login node

Login nodes are kind of like your home base. You **DO NOT** run actual computations on the login node. Every time you run non-trivial compute on the login node, an angel loses its wings, so, you know, just keep that in mind. 

To log in to a login node (your access point to Sherlock), we'll use `ssh` from the command line. Fire up your shell and enter the line below:

```
$ ssh <SUNet ID>@login.sherlock.stanford.edu
```

If everything goes to plan, you'll see this:

```
<SUNet ID>@login.sherlock.stanford.edu's password:
```

Follow your new computational overlord's directions and nobody will get hurt. (Just enter your SUNet password.) If you guess your password correctly, you'll see this:

```
Enter a passcode or select one of the following options:

 1. Duo Push to XXX-XXX-1732
 2. Phone call to XXX-XXX-1732
 3. SMS passcodes to XXX-XXX-1732

Passcode or option (1-3):
```

where `1732` will be replaced with the last four digits of your cell phone number. Select an option and handle the 2FA and finally you'll finally be in. Now just keep quiet and nobody will even know you're here...

```
Success. Logging you in...
Last failed login: Sat Jul 27 17:20:19 PDT 2019 from 171.66.27.244 on ssh:notty
There were 2 failed login attempts since the last successful login.

             --*-*- Stanford Research Computing Center -*-*--
                ____  _               _            _
               / ___|| |__   ___ _ __| | ___   ___| | __
               \___ \| '_ \ / _ \ '__| |/ _ \ / __| |/ /
                ___) | | | |  __/ |  | | (_) | (__|   <
               |____/|_| |_|\___|_|  |_|\___/ \___|_|\_\ 2.0

-----------------------------------------------------------------------------
  This system is for authorized users only and users must comply with all
  Stanford computing, network and research policies. All activity may be
  recorded for security and monitoring purposes. For more information, see
  https://doresearch.stanford.edu/policies/research-policy-handbook and
  https://acomp.stanford.edu/about/policy
-----------------------------------------------------------------------------
  Sherlock is *NOT* approved for storing or processing HIPAA, PHI, PII nor
  any kind of High Risk data. Users are responsible for data compliance.
  See https://uit.stanford.edu/guide/riskclassifications for details.
-----------------------------------------------------------------------------

        Docs         https://www.sherlock.stanford.edu/docs
        Support      https://www.sherlock.stanford.edu/docs/#support

        Web          https://www.sherlock.stanford.edu
        News         https://news.sherlock.stanford.edu
        Status       https://status.sherlock.stanford.edu

-----------------------------------------------------------------------------

 Sherlock    status   | OPERATIONAL | uptime : 99.924%
             usage    |      79.39% | use/tot: 1,483/1,868 cores

 tshimko     cur.jobs | 0 RUNNING (0 core), 0 PENDING (0 core)
             job wait | 10 minutes in normal

+---------------------------------------------------------------------------+
| Disk usage for user tshimko (group: pfordyce)                             |
+---------------------------------------------------------------------------+
|   Filesystem |  volume /   limit                  | inodes /  limit       |
+---------------------------------------------------------------------------+
          HOME |    0.0B /  15.0GB [            0%] |      - /      - (  -%)
    GROUP_HOME | 301.4GB /   1.0TB [||         29%] |      - /      - (  -%)
       SCRATCH |   8.0KB /  20.0TB [            0%] |    2.0 /  20.0M (  0%)
 GROUP_SCRATCH |  48.0KB /  30.0TB [            0%] |   12.0 /  30.0M (  0%)
+---------------------------------------------------------------------------+
```

See this --> `There were 2 failed login attempts since the last successful login.`? That means I was dumb and forgot my password. Twice. Don't be like me.

## Preparing a job for submission

Alrighty, moving on. Now that we're here, let's compute! We're all [computational geniuses](https://media.giphy.com/media/3o6ZsWTFIZwuEbv2nK/giphy.gif) after all, right?

To submit a job to the cluster, we first have to make a submission script.

```
#!/bin/bash
#
#SBATCH --job-name=e_coli_alignment
#
#SBATCH --time=20:00
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=4G

#Load the modules we need
module load biology
module load bwa

bwa mem e_coli.fasta.gz e_coli_miseq_R1.fastq.gz e_coli_miseq_R2.fastq.gz > e_coli_aligned.sam
```

We can then submit this script by running the command below:

```
$ sbatch submit.sh
````
