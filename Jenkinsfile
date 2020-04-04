
node {
    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "artifactory"
    // Create an Artifactory Maven instance.
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
   
	// slackSend(channel: '#general', message: 'Message from Jenkins Pipeline1')
	
 rtMaven.tool = "maven"

    stage('Clone sources') {
        git url: 'https://github.com/biswast-user/WebApp'
    }

    stage('Artifactory configuration') {
        // Tool name from Jenkins configuration
        rtMaven.tool = "maven"
        // Set Artifactory repositories for dependencies resolution and artifacts deployment.
        rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    }

    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install'
    }

    stage('Publish build info') {
        server.publishBuildInfo buildInfo
    }

	// Slack message
	
slackSend channel: '#general', message: 'This is primary pipeline'
	
// slackSend(channel: "#general", message: "Here is the primary message")
	
// slackSend(channel: "#newtest", message: "https://www.nytimes.com", sendAsText: true)
	
 // slackSend(channel: '#general', color: 'good', message: 'Message from Jenkins Pipeline2')
	
// def slackResponse = slackSend(channel: "#general", message: "Here is the primary message")	
// slackResponse.addReaction("thumbsup")
	
// def slackResponse = slackSend(channel: "#general", message: "Here is the primary message")
// slackSend(channel: slackResponse.channelId, message: "Update message now", timestamp: slackResponse.ts)
	
// def slackResponse = slackSend(channel: '#general', message: "Started build")
// slackSend(channel: slackResponse.threadId, message: "Build still in progress")
// slackSend(
//    channel: slackResponse.threadId,
//    replyBroadcast: true,
//    message: "Build failed. Broadcast to channel for better visibility."
// )
	
    }
	 
