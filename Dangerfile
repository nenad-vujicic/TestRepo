
# Add orange color label to this Pull Request if number of changed lines is > 500
pr_number = github.pr_json["number"]
auto_label.set(pr_number, "Big PR", "FFC90E") if git.lines_of_code > 500

# Warn when there is a big PR
warn('Number of updated lines of code is too large to be in one PR. Perhaps it should be separated into two or more?') if git.lines_of_code > 500

# Define the folder path where your YAML files are located
yaml_folder = 'config/locales'

# Check if any YAML files other than en.yml are modified
modified_yaml_files = git.modified_files.select do |file|
  file.start_with?(yaml_folder) && File.extname(file) == '.yml' && File.basename(file) != 'en.yml'
end

# Report a warning if any other YAML files are modified
unless modified_yaml_files.empty?
  modified_files_str = modified_yaml_files.map { |file| "`#{file}`" }.join(', ')
  warn("The following YAML files other than `en.yml` have been modified: #{modified_files_str}. Only `en.yml` is allowed to be changed.")
  auto_label.set(pr_number, "Touched translations", "00C9FF")
end
