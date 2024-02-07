          - os: macos-11
            c_compiler: clang
            cpp_compiler: clang++
            artifact_name: ${{ github.event.repository.name }}
            asset_name: ${{ github.event.repository.name }}-macos-amd64


https://github.com/actions/runner/issues/644
