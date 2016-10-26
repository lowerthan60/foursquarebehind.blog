---
title: Multiple SSH Keys settings for different github account
date: 2016-10-24 11:57:10
tags:
- GitHub
- Tools
---

## (GitHub-Flavored) Markdown Editor##

1. [Generating a new ssh key and adding it to the ssh agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)

	specify the key store name

	```
 	ssh-keygen.exe -t rsa -b 4096 -f ~/.ssh/foursquarebehind -C "foursquarebehind@gmail.com"
	```

	Note: have to make sure the ssh daemon is started

	```
	eval "$(ssh-agent -s)"
	```


2. [Adding a new ssh key to your github account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)
	
3. Create a SSH config file (if there is not)

	location ~/.ssh/config
	E.g.

	```
	#lowerthan60 account
	Host github.com-lowerthan60
	HostName github.com
	User git
	IdentityFile ~/.ssh/lowerthan60
	
	
	#foursquarebehind account
	Host github.com-foursquarebehind
	HostName github.com
	User git
	IdentityFile ~/.ssh/foursquarebehind
	```


4. Git configuration
Navigate to the repository configuration file (REPO_INSTALLDIR/.git).
Open the config file with your favorite editor.
Locate the url value in the [remote "origin"] section
change the HTTPS style URL to git
E.g.
From :
```
[remote "origin"]
  fetch = +refs/heads/*:refs/remotes/origin/*
  url = https://newuserme@bitbucket.org/newuserme/bb101repo.git
```

To :
	
```  
[remote "origin"]
  fetch = +refs/heads/*:refs/remotes/origin/*
  url = git@personalid:newuserme/bb101repo.git
```
	
5. Commit [here it should be the 5th step but it shows starting from 1, it might be a bug of hexo]

	May have to re-config the user email and name 
	e.g. :

	``` 
	git config --global user.email "dhoerl@xyz.com"
	git config --global github.user "dhoerl"    
	```

	then add the changes and check in
	e.g.:

	```
	git add .
	git commit -m "update"
	git push origin master
	```

6. A sample script
	
	```
	# my github script
	cd ~/.ssh
	rm id_rsa
	rm id_rsa.pub
	rm config
	
	ln git_dhoerl id_rsa
	ln git_dhoerl.pub id_rsa.pub
	ln config_dhoerl config
	
	git config --global user.email "dhoerl@xyz.com"
	git config --global github.user "dhoerl"        
	# git config --global github.token "whatever_it_is" # now unused
	```

Note:
* generate ssh key for each account and add the key into the account's profile in github.
* make sure ssh-agent is started.
* may have to re-config user email and name when switch between github accounts.
* may have to be in gitbash because of in the normal windows terminal the ssh and git env may not be configured properly.
* make sure use the git style repository URL to replace the default HTTPS style URL.

TODO:
1. create a new post on foursquarebehind.github.io and summarize the steps properly.
2. try works with multiple git like repositories like bit-bucket and github together. 
 
Refrences:
* https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/
* https://confluence.atlassian.com/bitbucket/configure-multiple-ssh-identities-for-gitbash-mac-osx-linux-271943168.html
* https://gist.github.com/jexchan/2351996
* http://stackoverflow.com/questions/7927750/specify-an-ssh-key-for-git-push-for-a-given-domain
* http://stackoverflow.com/questions/3225862/multiple-github-accounts-ssh-config