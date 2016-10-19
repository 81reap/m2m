#!/bin/bash

# local variables
version=0.1

# quick manual
usage(){
  echo "Usage: m2m <-h|--help> <-v|--version> <-t|--token> <token from medium>"
  echo "           <-r|--remove> <path to markdown file>"
  echo ""
  echo "Options:"
  echo "  -h, --help		Output usage info"
  echo "  -t, --token		Set the user token (only needs to be set once)"
  echo "  -v, --version		Check the version of m2m"
  echo "  -r, --remove		Remove the token (token can be overriden with -t)"
  echo ""
  echo "Source: "
  echo "Bugs: "
}

# saving OAUTH token in .bash_profile or .profile
sToken(){
  if test -z $1;then
    echo "No token specified"
    exit 1
  elif test -z ${TOKEN};then
    echo "export TOKEN=$1" >>~/".bash_profile"
    source ~/".bash_profile"
    echo "Token set"
  elif test "$TOKEN" == "$1";then
    echo "That token is already set"
  elif test "$TOKEN" != "$1";then
    sed -i "" "/export TOKEN=$TOKEN/c\\
      export TOKEN=$1"  ~/".bash_profile"
    source ~/".bash_profile"
    echo "Token updated"
  else
    usage
    exit 1
  fi
}

# uploads the Markdownfile
uFile(){
  if test -z $TOKEN;then
    echo "Token not set"
    usage
    exit 1
  elif [[ $1 == *.md && -f $1 ]];then
    echo "file found"
    value=$(<$1)
    curl -X POST -i -H "Content-type: application/json" -c cookies.txt -X POST api.medium.com/v1/me -d '{"Authorization":" Bearer 181d415f34379af07b2c11d144dfbe35d"}'
  else
    echo "$1 is not a valid markdown file"
    exit 1
  fi
}

# loop to the arguments
while [[ $# -gt 0 ]]
do
    
  case $1 in
# help
    -h|--help)
    usage
    ;;
# version
    -v|--version)
    echo "v$version"
    ;;
# token
    -t|--token)
    shift # move onto the token
    sToken $1
    ;;
# remove
    -r|--remove)
    sed -i "" "/export TOKEN=$TOKEN/c\\
      " ~/".bash_profile"
    ;;
# file
    *)
    uFile $1
    ;;
  esac
  shift
done
