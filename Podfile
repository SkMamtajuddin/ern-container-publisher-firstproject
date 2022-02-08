require_relative './node_modules/react-native/scripts/react_native_pods'
require_relative './node_modules/@react-native-community/cli-platform-ios/native_modules'

platform :ios, '11.0'



target 'ElectrodeContainer' do
  config = use_native_modules!

  use_react_native!(
    :path => "./node_modules/react-native"
  )


  post_install do |installer|
    react_native_post_install(installer)

    # Workaround for FBReactNativeSpec dependency cycle issue
    # https://github.com/facebook/react-native/issues/31034#issuecomment-812564390
    # https://github.com/electrode-io/electrode-native-manifest/pull/217
    installer.pods_project.targets.each do |target|
      target.build_configurations.each do |config|
        config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
      end

      if (target.name&.eql?('FBReactNativeSpec'))
        target.build_phases.each do |build_phase|
          if (build_phase.respond_to?(:name) && build_phase.name.eql?('[CP-User] Generate Specs'))
            target.build_phases.move(build_phase, 0)
          end
        end
      end
    end
  end
end
