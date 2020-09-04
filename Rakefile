# frozen_string_literal: true

require 'rake'
require 'yaml'
require 'erubi'
require 'tilt'
require 'fileutils'

Configuration = Struct.new(:prefix, :container_name, :configurations_file, :template_name)
jenkins = Configuration.new('jenkins', 'jenkins-worker', 'configurations.yml', 'Dockerfile.jenkins.erb')
gitlab = Configuration.new('gitlab', 'gitlab-ci-worker', 'configurations.yml', 'Dockerfile.gitlab.erb')
CONFIGS = [jenkins, gitlab].freeze

def load_configurations(config)
  file_content = YAML.load_file(File.join(__dir__, config.configurations_file))
  file_content.reject { |key| key.start_with? '_' }
end

def container_name_plus_tag(config, tag)
  "dkdde/#{config.container_name}:#{tag}"
end

def folder_name(config, tag)
  [config.container_name, tag].join('__')
end

desc 'start the whole compilcation process'
task :default do
  Rake::Task['build'].invoke
  Rake::Task['compile'].invoke
end

desc 'create Dockerfiles'
task :build do
  CONFIGS.each do |config|
    load_configurations(config).each do |key, values|
      puts
      puts "Container: #{container_name_plus_tag(config, key)}"
      FileUtils.mkdir_p folder_name(config, key)
      variables = values.merge(
        dockerhub_organisation: 'dkdde',
        container_name: config.container_name,
        tag: key
      )
      template = Tilt.new(File.join(__dir__, config.template_name))
      dockerfile_content = template.render nil, variables
      File.write(File.join(__dir__, folder_name(config, key), 'Dockerfile'), dockerfile_content)
    end
  end
end

desc 'compile Dockerfiles to Docker images'
task :compile do
  CONFIGS.each do |config|
    load_configurations(config).each_key do |key|
      puts
      puts "Container: #{container_name_plus_tag(config, key)}"

      unless system("docker build -t #{container_name_plus_tag(config, key)} #{folder_name(config, key)} --no-cache")
        # experimental: --compress --squash
        raise('Premature exit due to error!')
      end
    end
  end
end

desc 'show Docker commmands to build images'
task :dryrun do
  CONFIGS.each do |config|
    load_configurations(config).each_key do |key|
      puts
      puts "Container: #{container_name_plus_tag(config, key)}"
      puts "  docker build -t #{container_name_plus_tag(config, key)} #{key}" # --compress --squash
      puts "  docker push #{container_name_plus_tag(config, key)}"
      puts "  docker pull #{container_name_plus_tag(config, key)}"
    end
  end
end

desc 'upload images to DockerHub'
task :upload do
  CONFIGS.each do |config|
    load_configurations(config).each_key do |key|
      puts
      puts "Container: #{container_name_plus_tag(config, key)}"
      system "docker push #{container_name_plus_tag(config, key)}"
    end
  end
end
