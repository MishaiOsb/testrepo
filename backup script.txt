#!/bin/bash

# This checks if the number of arguments is correct
# If the number of arguments is incorrect ( $# != 2) print error message and exit
if [[ $# != 2 ]]
then
  echo "backup.sh target_directory_name destination_directory_name"
  exit
fi

# This checks if argument 1 and argument 2 are valid directory paths
if [[ ! -d $1 ]] || [[ ! -d $2 ]]
then
  echo "Invalid directory path provided"
  exit
fi

targetDirectory=$1
destinationDirectory=$2

echo "The $1 is always the target"
echo "The $2 is always the destination"

currentTS=$(date +%s)

backupFileName="backup-[$currentTS].tar.gz"
origAbsPath=`/home/project`

cd # <-
destDirAbsPath=`/home/theia`

cd /home/project # <-
cd $1 # <-

yesterdayTS=$(($currentTS - 24 * 60 * 60))

declare -a toBackup

for file in $(ls) # [TASK 9]
do
  if ((`date -r $file +%s` > $yesterdayTS))
  then toBackup+=($file)
  fi
done
