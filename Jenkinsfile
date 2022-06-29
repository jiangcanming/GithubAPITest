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
	final String prTitleValidRegx = "(?i)\\[.*\\]\\[.*\\]\\[.*\\].*"
	final String prTitleMatchMilestoneRegx = "\\*\\[([0-9]+\\.[0-9]+\\.[0-9]+)\\].*"

	def hasInvalidTitleLabel = pullRequest.labels.contains(invalidTitleLabel)
	def isValidTitle = pullRequest.title ==~ prTitleValidRegx

	def matcher = pullRequest.title =~ prTitleMatchMilestoneRegx
	String matchedMilestone = ""
	if (matcher) {
		println matcher
		matchedMilestone = matcher[0][1]
		println "matchedMilestone: " + matchedMilestone
	}
	if (matchedMilestone == "" || matchedMilestone == null) {
		isValidTitle = false
	}

	println "hasInvalidTitleLabel: ${hasInvalidTitleLabel}"
	println "isValidTitle: ${isValidTitle}"

	if (hasInvalidTitleLabel && isValidTitle) {
		pullRequest.removeLabel(invalidTitleLabel)
		// TODO: 删除评论
	}

	if (!hasInvalidTitleLabel && !isValidTitle) {
		pullRequest.addLabels([invalidTitleLabel])
	}

	if (matchedMilestone == "" || matchedMilestone == null) {
		return
	}

	println pullRequest.milestone

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