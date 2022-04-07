# githooks

## prepare-commit-msg 

COMMIT_MSG_FILE="$1"

RED=$(tput setaf 1)
GREEN=$(tput setaf 2)


commit_contents=$(cat ${COMMIT_MSG_FILE})

if ! grep -qE '^((build|chore|ci|docs|feat|fix|perf|refactor|revert|style|test)(\(\w+\))?(!)?(: \S(.*\s*)*))|((Merge|Revert|Translated) (.*\s*)*)|(Initial commit$)' "$COMMIT_MSG_FILE" ;
then
  echo "\n${RED}There is something wrong with your commit mesage"
  echo "${RED}commit message regex: ${GREEN}^((build|chore|ci|docs|feat|fix|perf|refactor|revert|style|test)(\(\w+\))?(!)?(: \S(.*\s*)*))|((Merge|Revert|Translated) (.*\s*)*)|(Initial commit$)"
  echo "${RED}You should modify your commit mesage and try again."
  exit 1
fi

## pre-commit-checkout

local_branch="$(git rev-parse --abbrev-ref HEAD)"
echo "${local_branch}"

valid_branch_regex="^(feature|bugfix|improvement|library|prerelease|release|hotfix)\/[a-z0-9._-]+$"

if [[ ! $local_branch =~ $valid_branch_regex ]]; then
echo "\nThere is something wrong with your branch name"
echo "Branch name should be like:"
echo "             bugfix/wrong_safety_item"
exit 1
fi
