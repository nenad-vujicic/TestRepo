# Validate if PR's history is linear, i.e. there are no merge-commits
linear_history.validate!

# Get PR number and initialize is_good_pr variable
pr_number = github.pr_json["number"]
is_good_pr = true

# Report if number of changed lines is > 500
is_big_pr = git.lines_of_code > 500
if is_big_pr
  warn("Number of updated lines of code is too large to be in one PR. Perhaps it should be separated into two or more?")
  auto_label.set(pr_number, "Big PR", "ffff00")
  is_good_pr = false
else
  auto_label.remove("Big PR")
end

# Get list of translation files (except en.yml) which are modified
modified_yml_files = git.modified_files.select do |file|
  file.start_with?("config/locales") && File.extname(file) == ".yml" && File.basename(file) != "en.yml"
end

# Report if some translation file (except en.yml) is modified
if modified_yml_files.empty?
  auto_label.remove("Compromised Translations")
else
  modified_files_str = modified_yml_files.map { |file| "`#{file}`" }.join(", ")
  warn("The following YAML files other than `en.yml` have been modified: #{modified_files_str}. Only `en.yml` is allowed to be changed.")
  auto_label.set(pr_number, "Compromised Translations", "ff0000")
  is_good_pr = false
end

# Report "Everything is fine!" if no warnings were generated
if is_good_pr
  message("Everything is fine!")
  auto_label.set(pr_number, "Good PR", "00ff00")
else
  auto_label.remove("Good PR")
end
