
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
	
	/**
 * Send notifications based on build status string
 */
def call(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus = buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"
  def details = """<p>${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
    <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>"""

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary)

  hipchatSend (color: color, notify: true, message: summary)

  emailext (
      to: 'bitwiseman@bitwiseman.com',
      subject: subject,
      body: details,
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}
	
// slackSend channel: '#general', message: 'This is primary pipeline'
	
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
	 
