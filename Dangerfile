# Example Dangerfile for a Ruby on Rails project

# Lint Ruby files using RuboCop
rubocop

# Check for changes in spec files
spec_files = git.modified_files + git.added_files
if spec_files.any? { |file| file.start_with?('spec/') }
  message('Remember to update tests!')
end
