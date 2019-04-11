### Sample fastlane script to upload your app to TestFlight

```py
desc "Push a new beta build to TestFlight with Prod Config"
lane :beta do
	ensure_git_status_clean
	increment_build_number(xcodeproj: "yourAppProj.xcodeproj")
	build_ios_app(
		scheme: "yourTargetScheme",
		configuration: "Release",
		export_method: "app-store"
	)
	upload_to_testflight(skip_waiting_for_build_processing: true)
	commit_version_bump(xcodeproj: "yourAppProj.xcodeproj")
	add_git_tag
	push_to_git_remote
end
```