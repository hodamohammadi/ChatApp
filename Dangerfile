# Sometimes it's a README fix, or something like that - which isn't relevant for
# including in a project's CHANGELOG for example
declared_trivial = github.pr_title.include? "#trivial"

# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
if github.pr_title.include? "[WIP]"
    warn("PR is classed as Work in Progress - DO NOT MERGE")
else
    # Android Lint
    Dir["*/build/reports/lint-results-debug.xml"].each do |file|
        android_lint.skip_gradle_task = true
        android_lint.report_file = file
        android_lint.lint
    end

    # Kotlin Lint
    Dir["*/build/reports/detekt/detekt.xml"].each do |file|
        checkstyle_format.base_path = Dir.pwd
        checkstyle_format.report file
    end
end

# Warn when there is a big PR
warn("Big PR") if git.lines_of_code > 500

# Mainly to encourage writing up some reasoning about the PR, rather than
# just leaving a title
if github.pr_body.length < 5
    warn("Please provide a summary in the Pull Request description")
end