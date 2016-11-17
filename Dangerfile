# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
warn("PR is classed as Work in Progress 👷") if github.pr_title.include? "[WIP]"

# Warn when there is a big PR
warn("Big PR 🐘") if git.lines_of_code > 500

# - > +
message("Good job on cleaning the code 👏") if git.deletions > git.insertions

# Ensure a clean commits history
if git.commits.any? { |c| c.message =~ /^Merge branch '#{github.branch_for_base}'/ }
  fail('Please rebase to get rid of the merge commits in this PR 💣')
end

# Only accept PRs to the develop branch
if github.branch_for_base != "develop" && !github.pr_title.include?("hotfix")
  fail "Please re-submit this PR to develop 🚃"
end
