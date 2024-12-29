---
draft: true
title: How i came up with this blog
---
# MIGRATION TO HUGO FOR BLOG
commands


```basharch/user_configuration.json
  501  hugo
  502  hugo new site .
  504  git init
  507  tldr hugo
  508  hugo new blog/test
  509  man hug
  510  man hugo
  511  man hugo new
  512  man hugo-new-content
  513  hugo new sad
  516  vim archetypes/default.md
  518  cp archetypes/default.md archetypes/blog.md
  519  vim archetypes/blog.md
  520  hugo new asd
  521  hugo new blog/prova.md
  522  vim content/blog/prova.md
  523  vim archetypes/blog.md
  524  gg
  525  vim .gitig
  526  gg
  530  rm content/blog/prova.md
  531  hugo new blog/prova.md
  534  ll
  535  hugo serve
  536  hugo serve
  539  hugo serve
  541  hugo serve
  543  cat public/index.xml
  544  hugo serve
  545  hugo server -D
  546  hugo server -D
  551  cat inde
  553  hugo build
  557  hugo --help
  558  hugo new --help
  559  mkdir layouts/_default
  561  hugo server -D
  570  bdprochot_off.sh
  575  hugo server -D
  593  tmx ~/scripts/
  594  cd layouts/
  595  tree
  597  mv _default/* .
  599  rm _default/
  600  rm _default/ -r
  617  hugo server -D
  618  mkdir layouts/blog
  619  mv layouts/page/single.html  layouts/blog/
  620  hugo server -D
  621  hugo server -D
  625  cat public/blog/prova/index.html
  626  hugo server -D
  627  hugo server -D
  628  hugo server -D
  632  rm -rf layouts/page/
  633  git add layouts/
  648  hugo server -D
  649  hugo server --disableFastRender
  656  hugo server --disableFastRender
  671  cat hugo.toml
  674  hugo server --disableFastRender
  675  hugo server --disableFastRender --debug
  676  hugo server --disableFastRender --debug | grep debug
  677  hugo server --disableFastRender --loglevel debug
  678  hugo server --loglevel debug
  679  hugo --loglevel debug server
  680  hugo serve
  681  hugo server --BuildDraft
  682  hugo server --BuildDrafts
  683  hugo serve
  684  hugo server -D
  688  cp layouts/baseof.html layouts/blog/
  689  hugo server -D
  690  git clone https://github.com/hugo-sid/hugo-blog-awesome.git themes/hugo-blog-awesome
  691  hugo server -D
  692  hugo server -D
  693  git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
  695  hugo server -D
  696  cat themes/hugo-blog-awesome/exampleSite/hugo.toml >> hugo.toml
  697  hugo server -D
  698  hugo server -D
  699  hugo server -D
  701  mv layouts/ pippo
  703  cd themes/
  705  cdh
  706  cd hugo-blog-awesome/
  712  cat archetypes/default.md
  718  cd ..
  719  cd ..
  722  mv pippo/ ../
  724  hugo new post/prova.nd
  725  hugo new post/prova.md
  726  cp ~/vaults/TIL/pages/N3DS_SETUP.md content/blog/
  729  cp ~/vaults/TIL/assets/3ds.jpg assets/
  730  cp ~/Pictures/Camera/avatar.jpg assets/
  731  cp --preserve=timestamps ~/vaults/TIL/pages/* content/blog/
  733  cd themes/
  734  rm -rf ananke/
  736  cd hugo-blog-awesome/
  738  ks ae
  745  rm archetypes/blog.md
  746  hugo new blog/sadsadasd.md
  760  for file in content/blog/*
  761  for file in content/blog/*; do
  762  tldr stat
  763  stat --format="%d" content/blog/N3DS_SETUP.md
  764  man stat
  765  stat --format="%w" content/blog/N3DS_SETUP.md
  766  cat content/blog/prova.md
  767  man stat
  768  for file in content/blog/*; do d="$(stat $file --format="%w" | awk '{print $1}')
echo sed -i '2date: $d' $file
done

  769  for file in content/blog/*; do d="$(stat $file --format="%w" | awk '{print $1}')"; echo sed -i '2date: $d' $file; done
  770  for file in content/blog/*; do d="$(stat $file --format="%w" | awk '{print $1}')"; sed -i "2date: $d" $file; done
  771  for file in content/blog/*; do d="$(stat $file --format="%w" | awk '{print $1}')"; sed -i "2i date: $d" $file; done
  772  rm content/blog/post/  -r
  773  for file in content/blog/*; do d="$(grep -e '^# ' $file  | awk '{$1=""; print $0}')"; echo sed -i "2i title: $d" $file; done
  774  cd content/
  776  cd blog/
  778  cd ..
  780  cd ..
  781  for file in content/blog/*; do d="$(grep -e '^# ' $file  | awk '{$1=""; print $0}')"; echo sed -i "2i title: $d" $file; done
  782  for file in content/blog/*; do d="$(grep -e '^# ' $file  | awk '{$1=""; print $0}')"; sed -i "2i title: $d" $file; done
  784  rm content/blog/prova.md
  785  rm content/blog/sadsadasd.md
  786  cd ~/vaults/TIL/
  788  cd pages/
  790  gg
  791   gg
  796  git log --follow --format=%ad --date default bash_automation/SNMP_V3_QUERY.md | tail -1
  797  git log --follow --format=%d --date default bash_automation/SNMP_V3_QUERY.md | tail -1
  798  git log --follow --format=%a --date default bash_automation/SNMP_V3_QUERY.md | tail -1
  799* git log --follow --format=%ad --date default bash_automation/SNMP_V3_QUERY.md
  800  git log --follow --format=%a --date default bash_automation/SNMP_V3_QUERY.md | tail -1
  801  git log --follow --format=%ad --date default bash_automation/SNMP_V3_QUERY.md | tail -1
  802  cd ~/projects/hugo_test/
  804  cp ~/vaults/TIL/assets/* assets/
  805  cp ~/vaults/TIL/assets/* assets/ -r
  808  cp assets/* -r static/
  813  cd static/
  815  mkdir images
  816  cp *  images/
  817  cp imgs/* images/
  818  rm -rf imgs/
  820  mv * images/
  823  historyt
  824  history
  825  history | wl-copy
```


```bash
for file in **/*.md; do d="$(git log --follow --format=%ad --date=iso  $file | tail -1 | awk '{print $1}')"; sed -i "s/date:.*/date: $d/g" "$HOME/projects/hugo_test/content/blog/$(basename $file)"; done
```

