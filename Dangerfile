# Remove all previously added labels
auto_label.remove("Big PR")
auto_label.remove("Compromised Translations")
auto_label.remove("Merge Commits")

# Report if number of changed lines is > 500
pr_number = github.pr_json["number"]
warn("Number of updated lines of code is too large to be in one PR. Perhaps it should be separated into two or more?") if git.lines_of_code > 500
auto_label.set(pr_number, "Big PR", "ffff00") if git.lines_of_code > 500

# Get list of translation files (except en.yml) which are modified
modified_yaml_files = git.modified_files.select do |file|
  file.start_with?("config/locales") && File.extname(file) == ".yml" && File.basename(file) != "en.yml"
end

# Report if some translation file (except en.yml) is modified
unless modified_yaml_files.empty?
  modified_files_str = modified_yaml_files.map { |file| "`#{file}`" }.join(", ")
  warn("The following YAML files other than `en.yml` have been modified: #{modified_files_str}. Only `en.yml` is allowed to be changed.")
  auto_label.set(pr_number, "Compromised Translations", "ff0000")
end

# Report if there are more than 1 commit in PR
warn("There are more than 1 commit in this pull request. Consider squashing commits to keep the history clean.") if github.pr_json["commits"] > 1
auto_label.set(pr_number, "Merge Commits", "ffaec9") if github.pr_json["commits"] > 1

# Report "Everything is fine!" if no warnings were generated
message("Everything is fine!") if git.lines_of_code <= 500 && modified_yaml_files.empty? && github.pr_json["commits"] == 1
auto_label.set(pr_number, "Good PR", "00ff00") if git.lines_of_code <= 500 && modified_yaml_files.empty? && github.pr_json["commits"] == 1
