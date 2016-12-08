has_app_changes = !git.modified_files.grep(@LS_APP_DIR).empty?
has_test_changes = !git.modified_files.grep(@LS_TEST_DIR).empty?
is_refactoring = github.pr_title.include?("refactor")

@CHANGELOG_FILE = "CHNAGELOG.md"

begin
  g = Git.open "."
  g.object(":#{@CHANGELOG_FILE}")
  has_changelog = true
rescue
  has_changelog = false
end

## Failures

# Ensure a clean commits history
if git.commits.any? { |c| c.message =~ /^Merge branch '#{github.branch_for_base}'/ }
  fail('Please rebase to get rid of the merge commits in this PR')
end

# Add a CHANGELOG entry for app changes
if has_changelog && has_app_changes && !is_refactoring && !git.modified_files.include?("CHANGELOG.md")
    fail("Please include a #{github.html_link(@CHAMNGELOG_FILE)} entry")
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

