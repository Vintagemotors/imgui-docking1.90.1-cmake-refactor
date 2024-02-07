          - os: macos-11
            c_compiler: clang
            cpp_compiler: clang++
            artifact_name: ${{ github.event.repository.name }}
            asset_name: ${{ github.event.repository.name }}-macos-amd64


https://github.com/actions/runner/issues/644
https://github.com/orgs/community/discussions/60820





      - name: Update brew (MacOS)
        if: matrix.os == 'macos-11'
        run: brew update
  
   #   - name: Upgrade brew (MacOS)
   #     if: matrix.os == 'macos-11'
   #     run: brew upgrade

      - name: Install pkg-config (MacOS)
        if: matrix.os == 'macos-11'
        run: brew install pkg-config 

      - name: Install glfw (MacOS)
        if: matrix.os == 'macos-11'
        run: brew install glfw 




      - name: Upload binaries to release (Linux - GCC)
        if: matrix.os == 'ubuntu-latest' && matrix.c_compiler == 'gcc'
        uses: svenstaro/upload-release-action@v2
        with:
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          file: /home/runner/work/imgui-docking1.90.1-cmake-refactor/imgui-docking1.90.1-cmake-refactor/build/example_glfw_opengl2
          asset_name: example_glfw_opengl2-linux-g++-amd64
          tag: ${{ github.ref }}-${{ github.sha }}
