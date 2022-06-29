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

	def invalidTitleLabel = "do-not-merge/invalid-pr-title"
	def hasInvalidTitleLabel = pullRequest.labels.contains(invalidTitleLabel)
	def isInvalidTitle = pullRequest.title ==~ "((?i)(clos(?:e[sd]?))|(fix(?:(es|ed)?))|(resolv(?:e[sd]?)))[\\s:]+(\\w+/\\w+)?#(\\d+)"
	def isValidTitle = pullRequest.title ==~ "(?i)\\[.*\\]\\[.*\\]\\[.*\\].*"
	def matcher = pullRequest.title =~ "*\\[([0-9]+\\.[0-9]+\\.[0-9]+)\\].*"

	println "hasInvalidTitleLabel: ${hasInvalidTitleLabel}"
	println "isInvalidTitle: ${isInvalidTitle}"
	println "isValidTitle: ${isValidTitle}"

	while(matcher.find()) {
		println matcher.group()
	}

	// if (hasInvalidTitleLabel && isCustomTitle && !isInvalidTitle) {
	// 	pullRequest.removeLabel(invalidTitleLabel)
	// 	// TODO：删除评论
	// }

	// if (isInvalidTitle || !isCustomTitle) {
	// 	pullRequest.addLabels([invalidTitleLabel])
	// 	// TODO: 添加评论
	// }

	// if groups == nil {
	// 	return
	// }

	// def milestone = groups[0]
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