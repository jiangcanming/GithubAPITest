#!groovy

@NonCPS
String getMileStoneFromText(String text) {
	def prTitleMatchMilestoneRegx = /.*\[([0-9]+\.[0-9]+\.[0-9]+)\].*/
	def matcher = text =~ prTitleMatchMilestoneRegx
 	return matcher ? matcher[0][1] : ""
}

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
	
	def hasInvalidTitleLabel = pullRequest.labels.contains(invalidTitleLabel)
	def isValidTitle = pullRequest.title ==~ prTitleValidRegx

	def matchedMilestone = getMileStoneFromText(pullRequest.title)
	println "matchedMilestone: " + matchedMilestone
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
	def jsonString = sh(script: """
		set -e
		api="https://api.github.com/repos/jiangcanming/GithubAPITest/milestones"
		curl -s \
			-H "Accept: application/vnd.github.v3+json" \
			\${api}
		""",
		returnStdout: true
	).trim()

	def milestonsMap = [:]
	try {
		def jsonSlurper = JsonSlurper()
		def milestoneList = jsonSlurper.parseText(jsonString)
		if (milestoneList != null && !milestoneList.isEmpty()) {
			for(m in milestoneList) {
				milestonsMap[m.title] = m.number
			}
		}
	} catch(Exception ex) {
		println "parse json fail, error: ${ex}"
	}
	
	def milestoneNumber = milestonsMap[matchedMilestone]
	println "milestoneNumber :" + milestoneNumber
	if (milestoneNumber) {
		pullRequest.milestone = milestoneNumber
	}
}