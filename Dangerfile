# frozen_string_literal: true

# Add orange color label to this Pull Request if number of changed lines is > 500
pr_number = github.pr_json["number"]
auto_label.set(pr_number, "Big PR", "FFC90E") if git.lines_of_code > 500

# Warn when there is a big PR
warn('Number of updated lines of code is too large to be in one PR. Perhaps it should be separated into two or more?') if git.lines_of_code > 500

# Add blue label to thtis Pull Request if abracadabra.txt file is changed
if git.added_files.include?('abracadabra.txt') || git.modified_files.include?('abracadabra.txt') || git.deleted_files.include?('abracadabra.txt')
  warn("README.md has been added, modified, or deleted. Please ensure changes are reviewed.")
  auto_label.set(pr_number, "Abracadabra Changed", "FF0000")
end
