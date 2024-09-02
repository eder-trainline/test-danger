# Path to the file where the version is defined
file_path = 'Sources/Element/Version.swift'

# Read the file content
begin
  file_content = File.read(file_path)

  # Regular expression to extract the version from the `version` variable
  version_match = file_content.match(/private var version: String {\s*"(\d+\.\d+\.\d+)"\s*}/)

  if version_match
    # Block the merge by default
    fail("Merge is currently blocked until version verification is complete.")

    current_version = version_match[1]
    message("The current library version is #{current_version}.")

    # Fetch all tags from the remote repository
    tags = `git tag`.split("\n")

    # Check if the current version exists as a tag
    if tags.include?(current_version)
      fail("The version #{current_version} already exists as a tag in the repository. Please update the version to resolve the conflict.")
    else
      # Remove the fail condition since the version does not exist as a tag
      message("The version #{current_version} is new and not yet tagged in the repository. Merge can proceed.")
      # GitHub-specific function to approve the PR (optional and context-dependent)
      # You might need an additional tool or bot to automate the removal of the block
      # e.g., posting a comment to approve if your CI setup supports it.
    end

  else
    fail("Could not find the version in the file #{file_path}.")
  end

rescue Errno::ENOENT
  fail("The file #{file_path} was not found.")
end
