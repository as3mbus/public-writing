---
Created: 2021-09-30T10:01
tags:
  - Management
  - Technical
  - Development
Last Edit: 2021-09-30T10:03
State: Done
---
**⚡Quick Steps**
1. [0. [Clone] one-time setup, cloning the repository](#0.%20%5BClone%5D%20one-time%20setup,%20cloning%20the%20repository)
1. [1. [Issue] Create New Issue in Project Git Page](#1.%20%5BIssue%5D%20Create%20New%20Issue%20in%20Project%20Git%20Page)
1. [2. [Merge Request] Create Merge Request for created issue](#2.%20%5BMerge%20Request%5D%20Create%20Merge%20Request%20for%20created%20issue)
1. [3. [Fetch] Accessing created branch](#3.%20%5BFetch%5D%20Accessing%20created%20branch)
1. [3. [Checkout] Accessing created branch](#3.%20%5BCheckout%5D%20Accessing%20created%20branch)
1. [4. [Add & Modify] Add/ Replace files accordingly](#4.%20%5BAdd%20&%20Modify%5D%20Add/%20Replace%20files%20accordingly)
1. [5. [Stage] stage the changes you want to commit](#5.%20%5BStage%5D%20stage%20the%20changes%20you%20want%20to%20commit)
1. [6. [Commit] Commit your changes](#6.%20%5BCommit%5D%20Commit%20your%20changes)
1. [7. [Push] Push your changes to remote](#7.%20%5BPush%5D%20Push%20your%20changes%20to%20remote)
1. [8. [Inform] Inform Programmer that the change have been pushed.](#8.%20%5BInform%5D%20Inform%20Programmer%20that%20the%20change%20have%20been%20pushed.)
1. [9. [Repeat] Repeat these steps](#9.%20%5BRepeat%5D%20Repeat%20these%20steps)

# 0. [Clone] one-time setup, cloning the repository

# 1. [Issue] Create New Issue in Project Git Page

![[Notion Personal/Attachments/Untitled 6.png|Untitled 6.png]]

Step 1.1. access issue page of the project

![[Untitled 1 3.png|Untitled 1 3.png]]

Step 1.2. Create New Issue

![[Notion Personal/Attachments/Untitled 2 2.png|Untitled 2 2.png]]

step 1.3. fill out issue information (don't forget to add label [2d /3d art], and click assign to me )

![[Untitled 3 2.png|Untitled 3 2.png]]

Result 1 Issue page created successfully

# 2. [Merge Request] Create Merge Request for created issue

if the project is forked. creating merge request from issue is not possible without removing the fork

![[Untitled 4 2.png|Untitled 4 2.png]]

Step 2.1. click down arrow at the side of `Create merge request` button. and add prefix to branch name `artist/` for example. the prefix might change as the project teams scale.

- **Create Merge Request Button Not Found ? Open The Toggle**
    
    ## 2.1 Creating Branch
    
    1. Open repository branch page
        
        ![[Untitled 5 2.png|Untitled 5 2.png]]
        
    2. click create new branch
        
        ![[Untitled 6 2.png|Untitled 6 2.png]]
        
    3. fill the branch name with format  
          
        `artist/<issue-number>-<issue-name>`  
        see screenshot for example  
        
        you can't use `space` in branch name use `-` instead
        
        ![[Untitled 7.png]]
        

# 3. [Fetch] Accessing created branch

open the cloned repository and Fetch remote changes

![[Untitled 8.png]]

# 3. [Checkout] Accessing created branch

open Remote

![[Untitled 9.png]]

# 4. [Add & Modify] Add/ Replace files accordingly

after checking out the branch you made.

add / modify the file you need.

it is done by simply moving, copy, paste, rename, replace, etc through your preferred file explorer

# 5. [Stage] stage the changes you want to commit

add assets into git staging area by checking / adding stuff you want to add into staged files section

![[Untitled 10.png]]

make sure to only stage file you actually changes. and avoid staging files that you don't directly modify

# 6. [Commit] Commit your changes

write summary of changes you did and commit your changes into local git repository

![[Untitled 11.png]]

Coordinate with repository maintainer in case there are some change that you shouldn't modify

# 7. [Push] Push your changes to remote

push your new commited branch to gitlab.

![[Untitled 12.png]]

# 8. [Inform] Inform Programmer that the change have been pushed.

Requirement : after you are done with the branch and ready to be merged with development branch.

1. inform the programmer to be checked.
2. programmer might ask some revision for the asset before merging.
3. you can push additional change to same branch until programmer informed you that the merge request are done.

# 9. [Repeat] Repeat these steps

after the branch is merged by programmer it will be deleted thus requiring you to make another issue

this means you will need to restart from step 1 next time you want to push another change.

if the branch isn't merged yet. you can skip to step 4 and add changes (commit) to previous branch