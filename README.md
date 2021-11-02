# devops-netology

Задание:
https://github.com/netology-code/sysadm-homeworks/blob/devsys10/02-git-01-vcs/README.md

### Gitignore

Добавим два файла `.gitignore`:
+ `./.gitignore` -- пуст и не делает ничего
+ `terraform/.gitignore` -- копия [Terraform.gitignore](https://github.com/github/gitignore/blob/master/Terraform.gitignore) которая приводит к игнорированию:
  +  содержимого директорий с именем `.terraform` по всему дереву
  +  в директории `terraform`:
    +  файлов с расширениями `.tfstate`, `.tfvars`, или содержащих `.tfstate.` в середине имени
    +  файлов с именами `crash.log`, `override.tf`, `override.tf.json`, `.terraformrc`, `terraform.rc`
    +  файлов имена которых оканчиваются на `_override.tf` или `_override.tf.json`


### Коммиты из IDE
1. первый 