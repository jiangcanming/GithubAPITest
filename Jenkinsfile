#!groovy

node {
	properties([
    	pipelineTriggers([
    		issueCommentTrigger('([\\s\\S]*)'),
        	pullRequestTrigger(pullRequestStates: ['opened', 'edited', 'reopened'])
    	])
	])

	if (env.GITHUB_COMMENT != null || env.CHANGE_ID == null) {
		return;
	}

	final String invalidTitleLabel = "do-not-merge/invalid-pr-title"
	def prTitleValidRegx = /(?i)\[.*\]\[.*\]\[.*\].*/
	def prTitleMatchMilestoneRegx = /.*\[([0-9]+\.[0-9]+\.[0-9]+)\].*/

	def hasInvalidTitleLabel = pullRequest.labels.contains(invalidTitleLabel)
	def isValidTitle = pullRequest.title ==~ prTitleValidRegx

	def matcher = pullRequest.title =~ prTitleMatchMilestoneRegx
	String matchedMilestone = ""
	if (matcher) {
		matchedMilestone = matcher[0][1]
		println "matchedMilestone: " + matchedMilestone
	}
	if (matchedMilestone == "" || matchedMilestone == null) {
		isValidTitle = false
	}

	println "hasInvalidTitleLabel: ${hasInvalidTitleLabel}"
	println "isValidTitle: ${isValidTitle}"

	if (isValidTitle) {
		if (hasInvalidTitleLabel) {
			pullRequest.removeLabel(invalidTitleLabel)
			// TDDO: 删除评论
		}
	} else {
		if (!hasInvalidTitleLabel) {
			pullRequest.addLabels([invalidTitleLabel])
		}
		// TODO: 添加评论
	}

	if (matchedMilestone == "" || matchedMilestone == null) {
		return
	}

	println "fetch milestones"
	// def jsonString = sh(script: """
	// 	set -e
	// 	api="https://api.github.com/repos/jiangcanming/GithubAPITest/milestones"
	// 	echo "api = \$api"
	// 	json=\$(curl -s \
	// 		-H "Accept: application/vnd.github.v3+json" \
	// 		\${api} | jq .)
	// 	echo "curl 结束"
	// 	echo \$json
	// 	""",
	// 	returnStdout: true
	// ).trim()

	// println jsonString

	// def milestoneMap = [:]
	// for (m in pullRequest.milestone) {
	// 	milestoneMap[m.title] = m.number
	// }
	// def milestoneNumber = milestoneMap[milestone]
	// if (milestoneNumber == null) {
	// 	return
	// }
	// pullRequest.milestone = milestoneNumber
}