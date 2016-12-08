has_app_changes = !git.modified_files.grep(@LS_APP_DIR).empty?
has_test_changes = !git.modified_files.grep(@LS_TEST_DIR).empty?
is_refactoring = github.pr_title.include?("refactor")

## Failures

# Ensure a clean commits history
if git.commits.any? { |c| c.message =~ /^Merge branch '#{github.branch_for_base}'/ }
  fail('Please rebase to get rid of the merge commits in this PR')
end

## Warnings

if has_app_changes && !has_test_changes && !is_refactoring
  warn "Tests were not updated"
end

# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
warn("PR is classed as Work in Progress") if github.pr_title.include? "[WIP]"

# Warn when there is a big PR
warn("Big PR") if git.lines_of_code > 500

# Warn if there's no description
warn("Please, provide a description to your PR") if github.pr_body.length < 20

## Messages

# - > +
message("Good job on cleaning the code") if git.deletions > git.insertions

