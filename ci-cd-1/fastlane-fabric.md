---
description: A sample fastlane script to upload an iOS app build to fabric
---

# fastlane fabric

#### Sample Fastlane Script to upload to fabric/crashlytics

```python
desc "Push a new beta build To Fabric with Dev Config"
lane :fabric do
    ensure_git_status_clean
    increment_build_number(xcodeproj: "yourAppProj.xcodeproj")
    build_ios_app(
        scheme: "appTargetScheme",
        configuration: "AdHoc",
        export_method: "ad-hoc"
    )
    commit = last_git_commit
    crashlytics(
        api_token: "API_TOKEN_HERE",
        build_secret: "build_secret_here",
        emails: "some@email.com, a@b.com, abc@xyz.com",
        notes: commit[:message]
    )
    commit_version_bump(xcodeproj: "yourAppProj.xcodeproj")
    add_git_tag
    push_to_git_remote
end
```

