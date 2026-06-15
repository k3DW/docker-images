# Copyright 2026 Braden Ganetsky
# Distributed under the Boost Software License, Version 1.0.
# https://www.boost.org/LICENSE_1_0.txt

FROM mcr.microsoft.com/windows/servercore:ltsc2022

ARG VS_VERSION
ENV VS_VERSION=${VS_VERSION}

LABEL org.opencontainers.image.source=https://github.com/k3DW/docker-images
LABEL org.opencontainers.image.description="Visual Studio ${VS_VERSION} Build Tools"

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Invoke-WebRequest \
    -Uri "https://raw.githubusercontent.com/k3DW/setup-msvc/8cfcf243de3632fee97eebefde7ecd7f97995769/install_msvc.ps1" \
    -OutFile .\install_msvc.ps1 \
  ; \
  try { \
    .\install_msvc.ps1 -inputVersion $VS_VERSION -installPath C:\vs-install; \
  } finally { \
    Remove-Item .\install_msvc.ps1; \
  }; \
  cmd /c 'C:\vs-install\VC\Auxiliary\Build\vcvars64.bat >nul && set' | \
  ForEach-Object { \
    if ($_ -match '^([^=]+)=(.*)$') { \
      [System.Environment]::SetEnvironmentVariable($Matches[1], $Matches[2], 'Machine') \
    } \
  };

ENTRYPOINT ["cmd.exe", "/s", "/c"]
CMD ["powershell.exe"]
