# Remove all previously added labels
auto_label.remove("Big PR")
auto_label.remove("Compromised Translations")

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

# Report "Everything is fine!" if no warnings were generated
message("Everything is fine!") if git.lines_of_code <= 500 && modified_yaml_files.empty?
auto_label.set(pr_number, "Good PR", "0000ff") if git.lines_of_code <= 500 && modified_yaml_files.empty?
