# Path to the file where the version is defined
file_path = 'Sources/Element/Version.swift'

# Read the file content
begin
  file_content = File.read(file_path)

  # Regular expression to extract the version from the `version` variable
  version_match = file_content.match(/private var version: String {\s*"(\d+\.\d+\.\d+)"\s*}/)

  if version_match
    current_version = version_match[1]
    message("The current library version is #{current_version}.")
  else
    warn("Could not find the version in the file #{file_path}.")
  end

rescue Errno::ENOENT
  fail("The file #{file_path} was not found.")
end
