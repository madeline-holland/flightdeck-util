# Testing

Due to the complex chains of dependencies on the Ansible k8s module, testing this container requires mocking up part of a full build.

## Procedure

While you can test with a full Docker Hub build and a complete site deploy, this can have a lot of turn around to solve individual issues. Instead, it's better to touch on the main components of a deploy.

To test:

1. Build the container as normal.
2. Open the `test-playbook.yml.example` file.
3. Run the container interactively.
4. Using `vim`, copy the contents of the example playbook into the container.
5. Edit the new file, replacing the Digital Ocean API key, cluster name, and cluster namespace.
6. Use `ansible-playbook` to deploy the contents.
7. If successful, delete the resulting secret, `auth-test-delete-later` in your kubernetes cluster in the provided namespace.

## Troubleshooting a successfully built container

Even if the container builds successfully, that's not assurance that the above test will past. Python links libraries (modules) at runtime, meaning some errors aren't exposed until attempting to test.

To troubleshoot the container:

1. Comment out the final `USER` statement in the Dockerfile. This will force the container to be built as root.
2. Repeat the testing procedure as stated above. It doesn't need root to operate, but it does give us full access to the container to install whatever we need.
3. When the problem is found, update the Dockerfile. Uncomment the `USER` line.
4. Rebuild the container.
5. Rerun the test.
