node {
	stage 'Checkout and Setup'
		checkout scm
		sh 'cd fastlane'
	stage 'Fastlane Lint'
		sh 'export LC_ALL=en_US.UTF-8 && chown -R root:jenkins target/ fastlane env && fastlane lint'
	stage 'Test'
		sh 'fastlane test'
	stage 'Build'
		def build_number = env.BUILD_NUMBER
		sh "fastlane build build_number:${build_number}"
	stage 'Deploy'
		archive 'reports/, dist/'
		sh 'fastlane deploy'
}