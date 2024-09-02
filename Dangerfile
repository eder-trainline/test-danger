# Path to the file where the version is defined
file_path = 'Sources/Element/Version.swift'

begin
  # Read the file content
  file_content = File.read(file_path)

  # Regular expression to extract the version from the `version` variable
  version_match = file_content.match(/private var version: String {\s*"(\d+\.\d+\.\d+)"\s*}/)

  if version_match
    current_version = version_match[1]
    message("Detected current library version: #{current_version}.")

    # Fetch all tags from the remote repository
    tags_output = `git tag --sort=-v:refname 2>&1`  # Capture output and errors from the git command, sorted by version
    if $?.success?
      tags = tags_output.split("\n")

      # Filter tags that are in the format of version numbers
      version_tags = tags.select { |tag| tag.match(/^\d+\.\d+\.\d+$/) }

      latest_tag = version_tags.first

      if latest_tag == current_version
        fail("Version conflict: The version #{current_version} already exists as a tag. Please update the `version` attribute in the `PaymentClientVersionProvider` to be higher than #{latest_tag}.")
      else
        # Convert tags to Gem::Version objects for comparison
        current_version_obj = Gem::Version.new(current_version)
        higher_versions = version_tags.select do |tag|
          Gem::Version.new(tag) > current_version_obj
        end

        if higher_versions.any?
          highest_version = higher_versions.map { |v| Gem::Version.new(v) }.max
          fail("Version conflict: Existing tags have versions greater than #{current_version}. The highest existing tag is #{highest_version}. Please update the `version` attribute in the `PaymentClientVersionProvider` to be greater than #{highest_version}.")
        else
          message("The version #{current_version} is new and has not been tagged in the repository. Merge can proceed.")
          message("Reminder: Please create a tag #{current_version} after the merge is completed.")
        end
      end
    else
      fail("Error: Unable to fetch tags from the repository. Details: #{tags_output}")
    end

  else
    fail("Error: Unable to extract the version from the file #{file_path}. Please ensure that the version format is correct.")
  end

rescue Errno::ENOENT
  fail("Error: The file #{file_path} was not found. Please ensure the file exists in the specified path.")
rescue => e
  fail("An unexpected error occurred: #{e.message}")
end
