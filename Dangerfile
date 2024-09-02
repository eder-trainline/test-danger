# Path to the file where the version is defined
file_path = 'Sources/Element/Version.swift'

begin
  # Read the file content
  file_content = File.read(file_path)

  # Regular expression to extract the version from the `version` variable
  version_match = file_content.match(/private var version: String {\s*"(\d+\.\d+\.\d+)"\s*}/)

  if version_match
    current_version = version_match[1]
    message("The current library version is #{current_version}.")

    # Fetch all tags from the remote repository
    tags_output = `git tag 2>&1`  # Capture output and errors from the git command
    if $?.success?
      tags = tags_output.split("\n")

      # Filter tags that are in the format of version numbers
      version_tags = tags.select { |tag| tag.match(/^\d+\.\d+\.\d+$/) }

      # Convert tags to Gem::Version objects for comparison
      current_version_obj = Gem::Version.new(current_version)
      higher_versions = version_tags.select do |tag|
        Gem::Version.new(tag) >= current_version_obj
      end

      if higher_versions.include?(current_version)
        fail("The version #{current_version} already exists as a tag in the repository. Please update the `version` attribute in the `PaymentClientVersionProvider` to a version higher than the latest tag.")
      elsif higher_versions.any?
        highest_version = higher_versions.map { |v| Gem::Version.new(v) }.max
        fail("There are existing tags with versions greater than or equal to #{current_version}. The highest existing tag is #{highest_version}. Please update the `version` attribute in the `PaymentClientVersionProvider` to a version greater than #{highest_version}.")
      else
        message("The version #{current_version} is new and not yet tagged in the repository. Merge can proceed.")
      end
    else
      fail("Failed to fetch tags from the repository. Error: #{tags_output}")
    end

  else
    fail("Could not find the version in the file #{file_path}. Please ensure the version format is correct.")
  end

rescue Errno::ENOENT
  fail("The file #{file_path} was not found. Please ensure the file exists in the correct path.")
rescue => e
  fail("An unexpected error occurred: #{e.message}")
end
