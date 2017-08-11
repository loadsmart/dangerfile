has_app_changes = !git.modified_files.grep(@LS_APP_DIR).empty?
has_test_changes = !git.modified_files.grep(@LS_TEST_DIR).empty?
is_test_exempt = ["[refactor]", "refactor:", "[chore]", "chore:"].any? { |label| github.pr_title.downcase.include?(label) }

@CHANGELOG_FILE = "CHANGELOG.md"

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

## Warnings

# Add a CHANGELOG entry for app changes
if has_changelog && has_app_changes && !is_test_exempt && !git.modified_files.include?("CHANGELOG.md")
    warn("Please include a #{github.html_link(@CHANGELOG_FILE)} entry")
end

if has_app_changes && !has_test_changes && !is_test_exempt
  warn "Tests were not updated"
end

# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
warn("PR is classed as Work in Progress") if github.pr_title.downcase.include? "[wip]"

# Warn when there is a big PR
warn("Big PR") if git.lines_of_code > (@LS_MAXIMUM_LINES_OF_CODE || 500)

# Warn if there's no description
warn("Please, provide a description to your PR") if github.pr_body.length < 20

## Messages

# - > +
message("Good job on cleaning the code") if git.deletions > git.insertions

